require 'bundler/gem_tasks'

require 'apple'
require 'rspec/core/rake_task'
require 'pry'

RSpec::Core::RakeTask.new(:spec)

task :default => :spec

task :define_helpers do
  def reload!
    $VERBOSE = nil

    files = $LOADED_FEATURES.select { |feat| feat =~ /\/apple\-media\// }
    files.each { |file| load file }

    true
  end

  def load_client
    Apple.configure do |config|
      config.secret_key = ENV['APPLE_SECRET_KEY']
      config.key_id     = ENV['APPLE_KEY_ID']
      config.team_id    = ENV['APPLE_TEAM_ID']
    end

    Apple::Client.new
  end
end

task load_client: :define_helpers do
  load_client
end

task generate_token: :define_helpers do
  puts load_client.token
end

task console: :define_helpers do
  ARGV.clear
  Pry.start
end
