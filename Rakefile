require "yaml"
require "liquid"
require "fileutils"

def generate_contract(client, folder)
  liquid2pdf client, "csv.tex", folder

  clean_all_except_pdf folder
end

def liquid2pdf(context, filename, out_folder)
  template = Liquid::Template.parse(File.read(filename))
  File.open("#{out_folder}/#{filename}", "w") do |f|
    f.write template.render(context)
  end
  `cd "#{out_folder}" && texi2pdf --batch #{filename}`
end

def clean_all_except_pdf(folder)
  `find "#{folder}" -type f -not -name '*pdf' | xargs rm`
end

task :generate_contracts do
  Dir["clients/*.yml"].each do |file|
    client = YAML.load_file(file)
    folder = "contrats/#{File.basename(file, ".*")}"
    FileUtils.mkdir_p folder

    generate_contract(client, folder)
  end
end

task :default => :generate_contracts