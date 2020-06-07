---
layout: post
title: "Uploading images via Etsy API with Ruby"
date: 2012-08-27 07:51
comments: true
categories:
---

As I was building the [photo genius app](http://photogenius.tailoredapps.co/) for [Tailored](http://tailoredapps.co/), I was tasked with uploading images to [Etsy](http://etsy.com) via its [API](http://www.etsy.com/developers). There is little documentation on how to do this. So I'm going to show you how to do it.

<!-- more -->

Etsy has an [api for image uploading](http://www.etsy.com/developers/documentation/reference/listingimage#section_uploading_images). But it requires a multipart form post. What if you stored your images somewhere like [S3](http://aws.amazon.com/s3/) where it is easier to access via a url? Can we upload an image via a url? The trick is to make use of Ruby's [OpenURI library](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/open-uri/rdoc/OpenURI.html) to open the file via url on the fly. The instructions below assumes you're on a Rails app.

First create a new file with the code below.

``` ruby
# config/initializers/open_uri_fix.rb
#
# Don't allow downloaded files to be created as StringIO. Force a tempfile to be created.
# For the image publisher
OpenURI::Buffer.send :remove_const, 'StringMax' if OpenURI::Buffer.const_defined?('StringMax')
OpenURI::Buffer.const_set 'StringMax', 0
```

This sets the constant of __StringMax__ in __OpenURI__ to be 0. The __StringMax__ constant is used to determine when to open the url as a __File__ or __StringIO__ object. If the file size is smaller than __StringMax__, __OpenURI__ will use __StringIO__. __StringIO__ will cause problems in the app as it is not compatible with multipart form posts.

Assuming you have the [etsy ruby gem](http://rubygems.org/gems/etsy) installed, here is how you could write the upload method.

``` ruby
# script/etsy_upload.rb
require 'open-uri'

def api_upload(listing_id, rank, replacement_image_url=nil, listing_image_id=nil)
  url = "/listings/#{listing_id}/images"
  params = {
    listing_id: listing_id,
    rank: rank,
    multipart: true
  }

  if replacement_image_url.present?
    params = params.merge({image: open(replacement_image_url)})
  end

  if listing_image_id.present?
    params = params.merge({listing_image_id: listing_image_id})
  end

  EtsyInterface::Client.post(self.etsy_shop, url, params)
end
```

And here is how you could build a thin wrapper to make API calls. We're assuming you have an etsy shop and etsy user object.

``` ruby
# lib/etsy_interface.rb
module EtsyInterface
  module Client

    # Public: Makes a request to Etsy API
    #
    # shop   - The EtsyShop instance
    # url    - The String which is the URL to query for
    # params - The Hash of attributes to pass in to the query
    #
    # Examples
    #
    #   EtsyInterface::Client.get('foo', shop, '/foo', {bar: 'bar'})
    #   # => {...}
    #
    # Returns the Hash
    #
    # Signature
    #
    #   <mtd>(args)
    #
    # mtd - Either of the get, post, put or delete HTTP methods
    %w(get put post delete).each do |mtd|
      define_method mtd do |shop, url, params={}|
        pre_setup
        Etsy::Request.send(mtd, url, access_params(shop, params)).to_hash
      end
    end

    protected

    # Public: Sets the constants of the Etsy class
    #
    # Examples
    #
    #   EtsyInterface::Client.pre_setup
    #   # => nil
    #
    # Returns nothing
    def pre_setup
      Etsy.environment = :production unless Rails.env == 'test'
      Etsy.api_key = Settings.etsy_api_key # your etsy api key
      Etsy.api_secret = Settings.etsy_secret # your etsy secret
    end

    # Public: Forms the query parameters for the request
    #
    # shop   - The EtsyShop instance
    # params - The Hash of attributes to pass in to the query
    #
    # Examples
    #
    #   EtsyInterface::Client.access_params(shop, {bar: 'bar'})
    #   # => {access_token: 'd', access_secret: 'g', bar: 'bar'}
    #
    # Returns the Hash
    def access_params(shop, params)
      user = shop.etsy_user
      access_params = {
        access_token: EtsyBuilder.etsy_oauth_token_for(user),
        access_secret: EtsyBuilder.etsy_oauth_secret_for(user)
      }.merge(params)
    end

  end
end
```

 Hope this helps!
