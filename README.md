# galleRy

Simple Rake task to generate an image gallery.

## Requirements
ruby, rake, RMagick, haml

## Usage

1. Copy `template.haml.example` to `template.haml`
2. Copy `config.yaml.example` to `config.yaml`
3. Put a bunch of images into the `i` directory
4. Run `rake` from the command line

You will now have a bunch of thumbnails in the `t` directory, plus an `index.html` file containing the thumbnails.

If you add new images to `i`, just run `rake` again & it will add them.

## Configuration

There are a few configuration options you may want to play around with in `config.yaml`; including image & thumbnail directories, template & output filenames, and maximum thumbnail width & height. Should be pretty self explanatory.

## Other tasks

- `rake convert_images` - just convert images in `i` to thumbnails
- `rake build_template` - just build the HTML file
- `rake clean` - remove all files in `t` & `index.html`
- `rake clobber` - same as `rake clean`, but removes all files in `i`, too

## TODO

- Pagination
- Make large image lightboxes linkable
