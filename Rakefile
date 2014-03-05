# vim: set ft=ruby:
# utf8
#
def hash_symbolize(hash)
  Hash[hash.keys.map(&:to_sym).zip(hash.values)]
end

def load_engines
  require 'haml'
  Dir.glob('templates/*.haml').inject({}) do |mem, path|
    matches = path.match(/\/(.*)\.haml/)
    key = matches[1]
    mem[key.to_sym] = Haml::Engine.new File.read path unless key.nil?
    mem
  end
end

def resume_data
  require 'yaml'
  YAML.load_file 'resume.yml'
end

desc "Builds an html file from the resume haml template"
task :build do 
  locals = hash_symbolize resume_data

  begin
    engines = load_engines
    main_engine = engines[:resume]
    blk = Proc.new do |what, obj|
      engine = engines[what]
      engine.render Object.new, hash_symbolize(obj), &blk unless engine.nil?
    end

    html = main_engine.render Object.new, locals, &blk
    File.open('resume.html', 'w') do |f|
      f.write html
    end
  rescue StandardError => e 
    $stderr.puts "Locals: " + locals.to_s
    raise e
  end
end

desc "Validates the resume YAML file"
task :validate do
  require 'yaml'
  begin
    YAML.load_file 'resume.yml'
    puts 'Valid'
  rescue StandardError => e
    message = "The resume yml is invalid:\n"
    message << e.message.lines.map do |line|
                 "  - " + line
               end.join("\n")

    $stderr.puts message
  end
end
