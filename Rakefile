
require 'RMagick'

task :default => [:convert_images, :build]

IMAGE_DIRECTORY = 'i'
THUMBNAIL_DIRECTORY = 't'
TEMPLATE_FILE = 'template.haml'
OUTPUT_FILE = 'index.html'
MAX_THUMBNAIL_WIDTH = 200
MAX_THUMBNAIL_HEIGHT = 200

def convert_images src_glob, target_directory, parent_task
    FileList[src_glob].each do |f|
	target = File.join target_directory, File.basename(f)
	file target => [f] do |t|
	    puts 'convert '+f + ' > ' + target

	    img = Magick::Image::read(f).first
	    thumb = img.resize_to_fit(MAX_THUMBNAIL_WIDTH, MAX_THUMBNAIL_HEIGHT)	    
	    thumb.write target

	end
	task parent_task => target
    end
end

task :build => OUTPUT_FILE
file OUTPUT_FILE => TEMPLATE_FILE do |t|
    puts "haml #{t.prerequisites[0]} > #{t.name}"
end

convert_images File.join(IMAGE_DIRECTORY, '*'), THUMBNAIL_DIRECTORY, :convert_images
