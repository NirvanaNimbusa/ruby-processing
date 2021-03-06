require_relative 'lib/ruby-processing/version'

def create_manifest
  title =  'Implementation-Title: rpextras (java extension for ruby-processing)'
  version =  format('Implementation-Version: %s', RubyProcessing::VERSION)
  file = File.open('MANIFEST.MF', 'w') do |f|
    f.puts(title)
    f.puts(version)
  end
end

task :default => [:init, :compile, :test, :gem]

desc 'Create Manifest'
task :init do
  create_manifest
end

desc 'Build gem'
task :gem do
  sh 'gem build ruby-processing.gemspec'
end

desc 'Compile'
task :compile do
  sh 'mvn package'
  sh 'mv target/rpextras.jar lib'
end

desc 'Test'
task :test do
  sh 'jruby test/vecmath_spec_test.rb'
  sh 'jruby test/deglut_spec_test.rb'
  sh 'jruby test/math_tool_test.rb'
  home = File.expand_path('~')
  config = File.exist?(format('%s/.rp5rc', home))
  if config
    sh 'jruby test/helper_methods_test.rb'
    ruby 'test/rp5_run_test.rb'
  else
    warn format('You should create %s/.rp5rc to run sketch tests', home)
  end
end

desc 'Clean'
task :clean do
  Dir['./**/*.%w{jar gem}'].each do |path|
    puts format('Deleting %s ...', path)
    File.delete(path)
  end
  FileUtils.rm_rf('./tmp')
end
