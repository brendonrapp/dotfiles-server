### TASKS ###

desc "Build everything!"
task :all do
  Rake::Task['git_submodules'].invoke
  Rake::Task['home'].invoke
  Rake::Task['bash'].invoke
  Rake::Task['bin'].invoke
  Rake::Task['vim'].invoke
end

desc "Init and update submodules"
task :git_submodules do
  system "git submodule init"
  system "git submodule update"
end

desc "create symlinks to the files in the user's home dir"
task :home do
  puts_blue "linking files"
  Dir["home/*"].each do |f|
    symlink_home("#{f}", ".#{File.basename f}")
  end
end

desc ".bash/ symlink"
task :bash do
  puts_blue "linking .bash/"
  symlink_home('bash', '.bash')
end

desc "~/bin symlink"
task :bin do
  puts_blue "linking ~/bin"
  symlink_home("bin", "bin")
end

desc ".vim/ symlink"
task :vim do
  puts_blue "linking .vim/"
  symlink_home('vim', '.vim')
  unless File.exists? "vim/backup"
    system "mkdir vim/backup"
  end
  symlink_home('vim/bundle/256-color', '.vim/colors')
  symlink_home('vim/pathogen/autoload', '.vim/autoload')
  puts_green ""
  puts_green "NOTICE: Remember to build Command-T module"
  puts_green "With RVM: rake command-t[rvm-version-number] (use MRI version that matches Ruby that your Vim was built with"
  puts_green "Without RVM: rake command-t"
end

desc "Compile Command-T plugin for vim"
task :"command-t", :rvm do |task, args|
  if File.exists? "vim/bundle/command-t"
    if args[:rvm].nil?
      if system"cd vim/bundle/command-t/ruby/command-t && rake make"
        puts_green "Command-T installed with current Ruby"
      end
    else
      if system "cd vim/bundle/command-t/ruby/command-t && rvm #{args[:rvm]} rake make"
        puts_green "Command-T installed with RVM Ruby: #{args[:rvm]}"
      end
    end
  else
    puts_red "Command T submodule not found, run `rake submodule` to fetch it"
  end
end

desc "Update all plugins in vim/bundle"
task :vim_update_plugins do
  dir = File.dirname(__FILE__)
  Dir["vim/bundle/*"].each do |n|
    puts "Updating #{n}"
    `cd #{n}; git checkout master; git pull; cd #{dir}`
  end
end

### UTILITY FUNCTIONS ###
# Heavily borrowed from https://github.com/csexton/dotfiles

def puts_red(str)
  puts "  \e[00;31m#{str}\e[00m"
end

def puts_green(str)
  puts "  \e[00;32m#{str}\e[00m"
end

def puts_blue(str)
  puts "  \e[00;34m#{str}\e[00m"
end

# Detect if this is a mac
def mac?
  RUBY_PLATFORM =~ /darwin/i
end

# Detect if this is linux
def linux?
  RUBY_PLATFORM =~ /linux/i
end

# Create a symlink in the user's home directory from the files in ./home
# src - file name of a file in .home/
# dest - name of the symlink to create in $HOME
def symlink_home(src, dest)
  home_dir = ENV['HOME']
  if( !File.exists?(File.join(home_dir, dest)) || File.symlink?(File.join(home_dir, dest)) )
    # FileUtils.ln_sf was making odd nested links, and this works.
    FileUtils.rm(File.join(home_dir, dest)) if File.symlink?(File.join(home_dir, dest))
    FileUtils.ln_s(File.join(File.dirname(__FILE__), src), File.join(home_dir, dest))
    puts_green "  #{dest} -> #{src}"
  else
    puts_red "  Unable to symlink #{dest} because it exists and is not a symlink"
  end
end 
