
userbin="#{ENV['HOME']}/bin"

task :default => [:install]
task :uninstall => [:remove]
task :install => [:test, :choosebin, :installfinal]
task :remove => [:test, :choosebin, :removefinal]

task :test do
  if RUBY_VERSION < "1.9"
    puts "Ruby 1.9 required."
    Kernel.exit -1
  end
  raise "Windows? Really?" unless ENV['HOME'] and Gem::Platform.local.os !~ /mswin/   #//cygwin etc should be immune
  raise "Can't detect HOME. Rake this yourself then, fancypants." unless ENV['HOME'] 
end

task :installfinal do

  if %x{uname -s} =~ /(Darwin|FreeBSD)/
    result=%x{sed 's/#!\/usr\/bin\/gawk -f/#!\/usr\/bin\/env gawk -f/' translate.awk | sudo tee #{userbin}/translate >/dev/null; chmod +x #{userbin}/translate}
    puts "Done. (result: #{result} )"
  else
    result=%x{cp translate.awk #{userbin}/translate}
    puts "Done. (result: #{result} )"
  end

end

task :removefinal do
  %x{rm -iv #{userbin}/translate}
end

task :choosebin do
  begin
    unless ENV['PATH'].to_s.match(userbin)
      puts "Looks like #{userbin} is not in your PATH.\nContinue anyway?" 
      unless gets.chomp.match(/[yY]{1}/)
        raise 'User abort.'
      end
    end
    unless File.writable?(userbin) and File::directory?(userbin) 
      raise userbin + 'doesn\'t seem like a writeable directory'
    end
  rescue Exception => msg
    puts msg
    puts "Do you want to try another directory?"
    result=gets.chomp
    if result =~ /[yY]{1}/
      puts "Okay, you can use any directory writeable by this user." 
      puts "Enter the full path:"
      userbin=gets.chomp
    else
      puts "Bye."
      Kernel.exit -1
    end
    retry
  end
end

# NOTE: upsteam has short tests for Japanese -- omit
#
