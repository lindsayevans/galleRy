
require 'yaml'

task :default => [:convert_images, :build]

IMAGE_DIRECTORY = 'i'
THUMBNAIL_DIRECTORY = 't'
TEMPLATE_FILE = 'template.haml'
OUTPUT_FILE = 'index.html'

def convert_images src_glob, target_directory, parent_task
    FileList[src_glob].each do |f|
	target = File.join target_directory, File.basename(f)
	file target => [f] do |t|
	    puts 'convert '+f + ' > ' + target
	end
	task parent_task => target
    end
end

task :build => OUTPUT_FILE
file OUTPUT_FILE => TEMPLATE_FILE do |t|
    puts "haml #{t.prerequisites[0]} > #{t.name}"
end

convert_images File.join(IMAGE_DIRECTORY, '*'), THUMBNAIL_DIRECTORY, :convert_images
