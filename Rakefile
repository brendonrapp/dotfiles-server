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

desc "create symlinks to the files in the user's home dir"
task :home do
  puts_blue "linking files"
  Dir["home/*"].each do |f|
    symlink_home("#{f}", ".#{File.basename f}")
  end
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
end

desc "Update all plugins in vim/bundle"
task :vim_update_plugins do
  dir = File.dirname(__FILE__)
  Dir["vim/bundle/*"].each do |n|
    puts "Updating #{n}"
    `cd #{n}; git checkout master; git pull; cd #{dir}`
  end
end

desc ".bash/ symlink"
task :bash do
  puts_blue "linking .bash/"
  symlink_home('bash', '.bash')
end
