require 'nokogiri'

task :default => :release

def read_ver()
  File.open('pom.xml', 'rb') do |f|
    doc = Nokogiri::XML(f.read())
    return doc.at_css('project>version').content
  end
end

ver = read_ver()
target_name = "maxpayne2starter-#{ver}"

task :package do
  sh "mvn package"
end

task :release => :package do
  release_dir = "release/#{target_name}"
  rm_rf 'release'
  mkdir_p release_dir
  cp 'res/maxpayne2starter.bat', release_dir
  cp "target/#{target_name}.jar", "#{release_dir}/maxpayne2starter.jar"
  cp 'README.md', release_dir

  cwd = pwd()
  cd release_dir
  sh <<-DOC.strip()
    7z a "#{target_name}.7z" "*"
  DOC
  mv "#{target_name}.7z", ".."
  cd cwd
end

task :local => :package do
  cp "target/#{target_name}.jar", 'c:/games/max payne 2/maxpayne2starter.jar'
end

task :debug =>:package do
  sh "java -jar target/#{target_name}.jar"
end
