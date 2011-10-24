DOTFILES = Dir.glob("*.dt")

HOME = Dir.home

HERE_PATH = ENV['PWD']

def dotfile_basename(file)
  File.basename(file, '.dt')
end

def home_path(file)
  File.join(HOME, ".#{file}")
end

def here_path(file)
  File.join(File.expand_path(File.dirname(__FILE__)), file)
end

def backup_here(file)
  data = File.read(file)
  name_here = File.basename(file)
  bak_path = File.join(HERE_PATH, "bak#{name_here}")
  File.open(bak_path, 'a') do |file|
    file.write data
  end
end

task :default do
  STDOUT.write <<-EOF
    This is my dotfile repo.
    Here are some important things to know:
    * Don't mess with the Rakefile
    * Run only `rake setup`, `rake cleanup`, or `rake full`
    * You can delete the bak.* files, they are only backup, by running rake cleanup.
    * You can add a new file to link by creating a *.dt file and running `rake setup` again
  EOF
end

desc "backs up the current rc files"
task :backup do
  DOTFILES.each do |file|
    file = dotfile_basename file
    STDOUT.write "Backing up #{file}\n"
    backup_here home_path file
  end
end

desc "symlinks files to $HOME/.filename"
task :link do
  DOTFILES.each do |file|
    file = dotfile_basename file
    STDOUT.write "Linking #{file} to #{home_path file}\n"
    # File.symlink(path_for(file), file)
  end
end

desc "backs the current dotfiles up and symlinks the .dt files to the home dir."
task :setup => [:backup, :link]

desc "deletes the backed-up (bak.*) files"
task :cleanup do
  Dir.glob('bak.*').each do |file|
    STDOUT.write "Deleting #{file}\n"
    File.delete file
  end
end

desc "runs the setup and cleanup steps"
task :full => [:setup, :cleanup]
