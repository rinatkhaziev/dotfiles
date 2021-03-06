DOTFILES_ROOT = File.dirname(__FILE__)

desc 'Install all dotfiles'
task :dotfiles do
  puts "Installing DOT files..."
  home = ENV['HOME']
  dotfiles_dir = "#{File.dirname(__FILE__)}/dotfiles"
  Dir["#{dotfiles_dir}/*"].each do |file|
    link_path = File.join(home, ".#{File.basename(file)}")
    system %x[unlink #{link_path}] if File.exists?(link_path)
    system %[ln -vsf #{file} #{link_path}]
  end
end

desc 'Install all bin files'
task :binfiles do
  source_path = File.join(File.dirname(__FILE__), 'bin')
  target_path = File.join(ENV['HOME'], '.bin')

  if File.exists?(target_path)
    puts "Unlinking installed bin directory"
    system %[unlink #{target_path}]
  end

  puts "Installing new bin directory"
  puts "ln -vsf #{source_path} #{target_path}"
  system %[ln -vsf #{source_path} #{target_path}]
end

desc 'Install default gems'
task :gems do
  puts "Installing GEM files..."
  File.readlines('default_gems').each do |gem|
    gem_name = gem.strip
    puts " -> #{gem_name}"
    `gem install #{gem_name}`
  end
end

desc 'Install multiple ruby versions'
task :rubies do
  %(1.8.7 1.9.2 1.9.3 ree rbx).each do |r|
    puts "Installing #{r}"
    `rvm install #{r}`
  end
end

namespace :sublime do
  desc 'Install settings'
  task :settings do
    root_path   = File.expand_path('~/Library/Application Support/Sublime Text 3')
    source_path = File.join(ENV['HOME'], '.sublime_text3', 'Preferences.sublime-settings')
    target_path = File.join(root_path, 'Packages/User/Preferences.sublime-settings')

    # Make sure we have sublime folder under user home
    if !File.exists?(source_path)
      system %[cp -a #{DOTFILES_ROOT}/sublime_text3 #{ENV['HOME']}/.sublime_text3]
    end

    # Unlink current settings
    if File.exists?(target_path)
      system %[unlink \"#{target_path}\"]
    end

    # Link new settings
    Dir.chdir File.dirname(__FILE__) do
      system %[ln -vsf #{source_path} \"#{target_path}\"]
    end
  end

  desc 'Install custom themes'
  task :themes do
    repo   = 'git://github.com/daylerees/colour-schemes.git'
    path   = File.join(ENV['HOME'], '.sublime-themes')
    target = File.expand_path('~/Library/Application Support/Sublime Text 2/Packages/Custom Themes')

    # Clone or update themes repository
    system %[git clone #{repo} #{path}] if !File.exists?(path)
    system %[cd #{path} && git pull]

    # Link to Sublime home dir
    system %[unlink \"#{target}\"] if File.exists?(target)
    system %[ln -vsf #{path} \"#{target}\"]
  end
end

namespace :git do
  desc 'Install git templates'
  task :templates do
    source_path = File.join(File.dirname(__FILE__), 'git_templates')
    target_path = File.join(ENV['HOME'], '.git_templates')

    if File.exists?(target_path)
      puts "Unlinking installed git_templates directory"
      system %[unlink #{target_path}]
    end

    puts "Installing new git_templates directory"
    puts "ln -vsf #{source_path} #{target_path}"
    system %[ln -vsf #{source_path} #{target_path}]
  end
end

desc 'Install default configuration'
task :install do
  Rake::Task['dotfiles'].invoke
  Rake::Task['binfiles'].invoke
end