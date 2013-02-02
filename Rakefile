require "fileutils"
require "sass"
require 'coffee-script'
require 'haml'

task :default => :build

desc 'Build _site content (all content, default task)'
task :build => [:clean, :css, :js, :haml, :pub] do
end

task :clean do
  FileUtils.mkdir_p "_site"
  FileUtils.rm_rf Dir.glob "_site/*"
end

task :pub do
  FileUtils.cp_r File.join('_pub', '.') , '_site'
  puts "Pub"
end

task :css do
  puts "CSS"
  ["holo"].each do |cssname|
    application_css = File.join "_css", "#{cssname}.sass"
    next unless File.exists? application_css
    sassengine = Sass::Engine.for_file(application_css, :syntax => :sass, :style => :compressed)
    css = sassengine.render

    File.open("_site/#{cssname}.css", "w") { |f| f.write(css) }
    puts "  #{cssname}"
  end
end

desc "JS"
task :js do
  puts "JS"
  ["holo"].each do |jsname|
    application_js = File.join "_js", "#{jsname}.coffee"
    next unless File.exists? application_js
    js = CoffeeScript.compile File.read application_js
    File.open("_site/#{jsname}.js", "w") { |f| f.write(js) }
    puts "  #{jsname}"
  end
end

desc "Haml"
task :haml do
  puts "HAML"
  ["index"].each do |hamlname|
    hamlengine = Haml::Engine.new File.read File.join "_html", "#{hamlname}.haml"
    html = hamlengine.render
    File.open("_site/#{hamlname}.html", "w") { |f| f.write(html) }
    puts "  #{hamlname}"
  end
end

desc 'Build site and deploy'
task :deploy => :build do
  sh "rsync --checksum -rtzh --progress --delete #{config["deploy_to"]} _save"
  sh "rsync --checksum -rtzh --progress --delete _site/ #{config["deploy_to"]}"
end

task :serve => :build do
  require 'serve'
  require 'serve/application'

  Serve::Application.run ["_site"]
end
