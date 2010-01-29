
require 'rake/clean'
require 'RMagick'
require 'haml'
require 'yaml'

task :default => [:convert_images, :build_template]

@config = YAML.load_file('config.yaml')

CLEAN.include("#{@config[:thumbnail_directory]}/*.*", @config[:output_file])
CLOBBER.include("#{@config[:image_directory]}/*.*")

def convert_images src_glob, target_directory, parent_task
    FileList[src_glob].each do |f|
	target = File.join target_directory, File.basename(f)
	file target => [f] do |t|
	    puts 'convert '+f + ' > ' + target

	    img = Magick::Image::read(f).first
	    thumb = img.resize_to_fit(@config[:max_thumbnail_width], @config[:max_thumbnail_height])	    
	    thumb.write target

	end
	task parent_task => target
    end
end

convert_images File.join(@config[:image_directory], '*'), @config[:thumbnail_directory], :convert_images

task :build_template do |t|
    puts "haml #{@config[:template_file]} > #{@config[:output_file]}"

    images = []

    FileList[File.join(@config[:thumbnail_directory], '*')].sort {|a,b|
	File.stat(b).mtime <=> File.stat(a).mtime
    }.each do |thumbnail|
	full_image = File.join @config[:image_directory], File.basename(thumbnail)
	images << {:thumbnail => thumbnail, :image => full_image}
    end

    template = File.read(@config[:template_file])
    engine = Haml::Engine.new(template, {:format => :html5, :attr_wrapper => '"'})
    output = engine.render(Object.new, :@config => @config, :images => images)
    fd = File.open(@config[:output_file], 'w')
    fd.write(output)
    fd.close

end

