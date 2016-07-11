"""
To use this script

1. Install rake and bundler 

```
  sudo gem install rake
  sudo gem install bundler
``` 

- Install gem dependencies

```
  cd path\to\WebDriverAgentSourceCode
  bundle install
```

- How to see all tasks:

```
  rake -T
```

"""
require 'rubygems'  

project_file = "ios-deploy.xcodeproj"
scheme_name = "ios-deploy"
configuration = "Release"
derivedDataPath = "./derivedData"
final_App_Name = "ios-deploy"
zipPath = "./#{final_App_Name}"

def run(command, min_exit_status = 0)
  puts "Executing: `#{command}`"
  system(command)
  return $?.exitstatus
end

def xcodebuild(flags , logfile)
  cmd = "set -o pipefail && xcrun xcodebuild #{flags} | tee #{logfile} | xcpretty"
  return run(cmd)
end

def cp(fromPath ,  toPath)
  cmd = "cp #{fromPath} #{toPath}"
  return run(cmd)
end

desc "Clean artifacts"
task :clean do
  run("rm -rf #{zipPath} && rm -rf #{derivedDataPath} && rm xcodebuild.log && rm xcodeipabuild.log")
end

desc "build .app and create .zip artifacts for real iOS Device"
task :build_app => [] do
    flags = "-project #{project_file} " \
            "-scheme #{scheme_name} " \
            "-configuration #{configuration} " \
            "-derivedDataPath #{derivedDataPath}"
            "clean build "
    build_status = xcodebuild(flags , 'xcodebuild.log')
    exit(-1) unless build_status==0
    build_status = cp("#{derivedDataPath}/Build/Products/#{configuration}/#{final_App_Name}" , "./")
    exit(-1) unless build_status==0
end


desc "default - build_archive"
task :default => ['clean' , 'build_app']  

