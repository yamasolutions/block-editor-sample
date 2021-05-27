# Block Editor for Ruby on Rails - Sample

This is a very basic sample host application for the [Block Editor Ruby on Rails gem](https://github.com/yamasolutions/block-editor)

It was created by following these steps;
* Create new Rails application
```
rails new block_editor_sample
```

* Generate Post scaffold
```
rails generate scaffold Post title:string
```

* Add Block Editor gem and bundle
```
// Gemfile
gem 'block_editor'
```

* Add Block Editor migrations & migrate
```
rails block_editor:install:migrations
rails db:migrate
```

* Add `include BlockEditor::Listable` to the `Post` model
```
class Post < ApplicationRecord
  include BlockEditor::Listable
end
```

* Add the block editor to `Post` form;
```
  <%= form.fields_for :active_block_list do |block_list| %>
    <%= BlockEditor::Instance.render(block_list) %>
  <% end %>
```

Updated trusted post parameters and specifically set the listable within the PostsController;
```
  # Only allow a list of trusted parameters through.
  def post_params
    params.require(:post).permit(:title, active_block_list_attributes: [ :id, :content ])
  end
```

```
@post.active_block_list.listable = @post
```

* Add the block editor Javascript and styles within `HEAD` tag
```
  <%= javascript_pack_tag 'block_editor/application', 'data-turbolinks-track': 'reload', webpacker: 'BlockEditor' %>
  <%= stylesheet_pack_tag 'block_editor/application', 'data-turbolinks-track': 'reload', webpacker: 'BlockEditor' %>
```

* Install Bootstrap. Note this is only required if you want to use the base styles that BlockEditor provides as they depend on Bootstrap
```
yarn add bootstrap
```

* Add SCSS imports for Bootstrap & base block editor styles. [Check them out here](https://github.com/yamasolutions/block-editor-sample/blob/master/app/assets/stylesheets/application.scss)

* Win
