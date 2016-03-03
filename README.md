# RailsCarrierwaveFocuspoint

CarrierWave extension for specifying focus point on image before they upload on server and cropped.

This gem based on [helper tool](http://jonom.github.io/jquery-focuspoint/demos/helper/index.html) of jquery.focuspoint javascript library (https://github.com/jonom/jquery-focuspoint)

## Installation

in Gemfile

```Ruby
gem 'rails-carrierwave-focuspoint'
```

## Usage

For using this gem you must add fields `focus_x:decimal` and `focus_y:decimal` to your db table used for storing CarrierWave picture information. 

```bash
rails g migration add_focuspoint_to_posts focus_x:decimal focus_y:decimal
```

```ruby
class AddFocuspointToPosts < ActiveRecord::Migration
  def change
    add_column :posts, :focus_x, :decimal
    add_column :posts, :focus_y, :decimal
  end
end
```

in CarrierWave uploader

```Ruby
class PhotoUploader < CarrierWave::Uploader::Base
  include CarrierWave::MiniMagick
  
  version :small do
    process crop_with_focuspoint: [100, 100]
  end
  
  version :big do
    process crop_with_focuspoint: [300, 200]
  end
end
```

in model

```Ruby
class Post < ActiveRecord::Base
  mount_uploader :photo, PhotoUploader
end
```

in .js.coffee file

```cofffeescript
#= require focuspoint_control
    
$ ->
  # Activate Focuspoint
  $("#post_photo").focuspoint()
```

in .css file

```css
*= require focuspoint_control
```

in view template
#TODO: FIGURE THIS PART OUT
```html.erb
form_for @post do |f|
  f.input :photo
  f.focuspoint_control :photo
```

## LICENSE

This project rocks and uses MIT-LICENSE.
