$LOAD_PATH << File.expand_path('../lib', __FILE__)

require 'solidus_extensions'
require 'circleci_solidus_extensions'
require 'pry'

Travis.access_token = ENV['TRAVIS_TOKEN']
CircleCi.configure { |config| config.token = ENV['CIRCLECI_TOKEN'] }

OLD_VERSIONS = %W[v1.0 v1.1 v1.2 v1.3 v1.4 v2.0 v2.1 v2.2 v2.3 v2.4]
VERSIONS = %W[v1.0 v1.1 v1.2 v1.3 v1.4 v2.0 v2.1 v2.2 v2.3 v2.4 v2.5 v2.6 v2.7 v2.8 v2.9 master]
PROJECTS = {
  # Auth
  'solidus_auth_devise' => { org: 'solidusio', ci: :circleci },
  'solidus_social' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_user_roles' => { org: 'boomerdigital' },

  # Payments
  'solidus_stripe' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_braintree' => { org: 'solidusio' },
  'solidus_paypal_braintree' => { org: 'solidusio' },
  'solidus_paypal_express' => { org: 'solidusio-contrib' },
  'solidus_affirm' => { org: 'solidusio-contrib' },
  'solidus_signifyd' => { org: 'solidusio' },
  'solidus_amazon_payments' => { org: 'boomerdigital' },
  'solidus_subscriptions' => { org: 'solidusio-contrib', ci: :circleci },

  # Shipping
  'solidus_easypost' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_shipstation' => { org: 'boomerdigital' },

  # I18n
  # 'solidus_i18n' => { org: 'solidusio' },
  'solidus_globalize' => { org: 'solidusio-contrib' },

  # Taxes
  'solidus_tax_cloud' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_avatax_certified' => { org: 'boomerdigital', branches: %w[master v2.1] },

  # Marketplace
  'solidus_multi_domain' => { org: 'solidusio-contrib', ci: :circleci },

  # Product and Variant Customization
  'solidus_asset_variant_options' => { org: 'solidusio' },
  'solidus_related_products' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_product_assembly' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_editor' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_email_to_friend' => { org: 'boomerdigital' },
  'solidus_flexi_variants' => { org: 'boomerdigital' },
  'solidus_handling_fees' => { org: 'solidusio-contrib' },

  # Search
  'solidus_elastic_product' => { org: 'boomerdigital' },
  'solidus_searchkick' => { org: 'solidusio-contrib', ci: :circleci },

  # Marketing
  'solidus_abandoned_carts' => { org: 'solidusio-contrib' },
  'solidus_recently_viewed' => { org: 'solidusio-contrib' },
  'solidus_product_feed' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_sitemap' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_trackers' => { org: 'solidusio-contrib', ci: :circleci },
  # 'solidus_seo' => { org: 'karmakatahdin' },

  # Promo
  'solidus_volume_pricing' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_wishlist' => { org: 'boomerdigital' },

  # Admin
  'solidus_prototypes' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_print_invoice' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_comments' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_reports' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_simple_dash' => { org: 'magma-labs' },

  # Log
  'solidus_papertrail' => { org: 'solidusio-contrib', ci: :circleci },
  'solidus_log_viewer' => { org: 'solidusio-contrib', ci: :circleci },

  # CMS
  'solidus_static_content' => { org: 'solidusio-contrib', ci: :circleci },

  # Law compliance
  'solidus_gdpr' => { org: 'solidusio-contrib', ci: :circleci },

  # Misc
  'solidus_geocoding' => { org: 'solidusio-contrib' },
  'solidus_reviews' => { org: 'solidusio-contrib' },

  # Legacy (extracted from core)
  'solidus_expedited_exchanges' => { org: 'solidusio-contrib', ci: :circleci },
}.map do |repo, options|
  case options[:ci]
  when :circleci
    SolidusExtensions::CircleCi::Project.new(options[:org], repo, options[:branches])
  else
    SolidusExtensions::Project.new(options[:org], repo, options[:branches])
  end
end.select(&:exists?)

task :retrigger do
  PROJECTS.each do |project|
    next unless project.name =~ /\Asolidusio/
    project.retrigger
  end
end

task :build do
  require 'erb'

  File.open('index.html', "w+") do |f|
    render = ERB.new(File.read("status.html.erb")).result
    f.write(render)
  end
end
