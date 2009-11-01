
require 'rake/clean'
require 'RMagick'
require 'haml'

# FIXME: getting a 'uninitialized constant Haml::Template' exception here
#Haml::Template.options[:format] = :html5
#Haml::Template.options[:attr_wrapper] = '"'

task :default => [:convert_images, :build_template]

IMAGE_DIRECTORY = 'i'
THUMBNAIL_DIRECTORY = 't'
TEMPLATE_FILE = 'template.haml'
OUTPUT_FILE = 'index.html'
MAX_THUMBNAIL_WIDTH = 150
MAX_THUMBNAIL_HEIGHT = 150

CLEAN.include("#{THUMBNAIL_DIRECTORY}/*.*", OUTPUT_FILE)
CLOBBER.include("#{IMAGE_DIRECTORY}/*.*")

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

convert_images File.join(IMAGE_DIRECTORY, '*'), THUMBNAIL_DIRECTORY, :convert_images

task :build_template do |t|
    puts "haml #{TEMPLATE_FILE} > #{OUTPUT_FILE}"

    config = {:max_thumbnail_width => MAX_THUMBNAIL_WIDTH, :max_thumbnail_height => MAX_THUMBNAIL_HEIGHT}

    images = []

    FileList[File.join(THUMBNAIL_DIRECTORY, '*')].each do |thumbnail|
	full_image = File.join IMAGE_DIRECTORY, File.basename(thumbnail)
	images << {:thumbnail => thumbnail, :image => full_image}
    end

    template = File.read(TEMPLATE_FILE)
    engine = Haml::Engine.new(template)
    output = engine.render(Object.new, :config => config, :images => images)
    fd = File.open(OUTPUT_FILE, 'w')
    fd.write(output)
    fd.close

end

