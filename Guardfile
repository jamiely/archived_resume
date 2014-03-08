# A sample Guardfile
# More info at https://github.com/guard/guard#readme

guard 'rake', :task => 'build:html' do
  watch(%r{^(resume.yml|templates/.*)})
end

guard 'rake', :task => 'build:css' do
  watch(%r{^(css/.*\.scss)})
end
