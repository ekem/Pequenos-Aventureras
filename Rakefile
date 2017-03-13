require 'net/http'
require 'lapis/minecraft/versioning'
require 'lapis/yggdrasil'
require 'rbnacl/libsodium'
require 'rbnacl'
require 'io/console'

task :default => [:latest]

desc "get lates client"
task :latest do
  list = Lapis::Minecraft::Versioning::VersionList

  uri = URI(list.official.latest_release.client_download.url)

  vendor_dir = 'vendor'
  Dir.mkdir('vendor')
  File.open(File.join(vendor_dir, 'client.jar'), 'wb') do |f|
    f.write(Net::HTTP.get(uri))
  end
end

desc "auth user"
task :auth, [:username, :password] do
  client = Lapis::Yggdrasil::AuthenticationClient.official
  session = client.authenticate(args[:username, :password)
end

# Take a string and encrypt
desc "encrypt"
task :encrypt, [:plaintext] do |t, args|
  encrypt 'test', 'this is the message'
end

desc "encrypt"
task :decrypt do
  decrypt
end

desc "run test"
task :test do
  some_test
end

def decrypt
  # TODO Write bytes appropriately
  #f = File.open('.pass', encoding: "BINARY")
  #1.times{f.gets}
  #salt = $_
  #2.times{f.gets}
  #password = $_
  #f.close

  key = File.read('.pass', encoding: 'BINARY')

  salt = key[0, RbNaCl::PasswordHash::Argon2::SALTBYTES]
  password = key[RbNaCl::PasswordHash::Argon2::SALTBYTES+1..-1]

  message = File.read('.message', encoding: 'BINARY')

  digest = RbNaCl::PasswordHash.argon2(password, salt, 5, 7_256_678, 32)

  #box = RbNaCl::SimpleBox.from_secret_key(digest)
  #puts box.decrypt(message)
end

def encrypt(password, message)
  # TODO Write bytes appropriately

  # If no password, get from stdin and hide input
  password.nil? do
    STDOUT.print "password: "
    STDOUT.flush
    password = STDIN.noecho(&:gets)
  end

  salt = RbNaCl::Random.random_bytes(RbNaCl::PasswordHash::Argon2::SALTBYTES)

  digest = RbNaCl::PasswordHash.argon2(password, salt, 5, 7_256_678, 32)

  File.open(".pass", "wb") do |f|
    f.write("#{salt.to_str}#{digest.to_str}")
  end

  box = RbNaCl::SimpleBox.from_secret_key(digest)

  ciphertext = box.encrypt(message)

  puts "#{$/} #{ciphertext}"

  File.open(".message", "wb") do |f|
    f.write(ciphertext.to_str)
  end
end

def some_test
  password = "testpassword"
  message = "Some test message."

  encrypt password, message
  sh 'cat .pass'
end
