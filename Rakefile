DOTFILES = ENV["DOTFILES"] || "~/.files"

task :rc do
  top_line = "export DOTFILES=\"#{File.expand_path DOTFILES}\"\n"
  extras = top_line + add_platform + "\n"
  File.write("home/.zshrc", extras + File.read(".zshrc"))
end

task :symlink do
  targets = FileList["home/.*"].exclude ['home/.ipython']
  targets.slice!(0, 2) #Get rid of . and ..

  targets.each do |f| 
    name = File.basename f
    full_path = File.join(Dir.pwd, f)
    target_path = File.expand_path "~/#{name}"
    FileUtils.ln_s(full_path, target_path, :force => true)
    puts "Symlinking #{target_path} #{f}"
  end

  IPY_PREFIX = '.ipython/profile_default'
  ipy_conf = File.join(Dir.pwd, "home/#{IPY_PREFIX}/ipython_config.py")
  ipy_link = File.expand_path "~/#{IPY_PREFIX}/ipython_config.py"
  FileUtils.ln_s(ipy_conf, ipy_link, :force => true)
end

task :default => [:symlink, :rc]

def add_platform
  if /darwin/.match RUBY_PLATFORM
   return "[ -e \"${DOTFILES}/source/.zdarwin\" ] && source \"${DOTFILES}/source/.zdarwin\""
  elsif /linux/.match RUBY_PLATFORM
   return "[ -e \"${DOTFILES}/source/.zlinux\" ] && source \"${DOTFILES}/source/.zlinux\""
  else 
    return ""
  end
end

