TestKitchen Image Prep
======================

You can make your TestKitchen runs faster by prepping your most used images with things that they will need installed:

1. Chef
2. Busser
3. ServerSpec

That way, they don't need to be installed over and over which wastes bandwidth and time.

To make TestKitchen not install Chef each time, add this to your `.kitchen.yml`:

    provisioner:
      require_chef_omnibus: false

This command assumes you've already got Chef installed.

Pre-installing the Serverspec and Busser gems can also help to shave minutes off of a test suite.

To use the pre-installed versions, add this to your `.kitchen.yml`:

    busser:
      root_path: /opt/busser

You'll need to use one of the updated images which pre-install them. Or add them yourself:

    export BUSSER_ROOT="/opt/busser"
    export GEM_HOME="/opt/busser/gems"
    export GEM_PATH="/opt/busser/gems"
    export GEM_CACHE="/opt/busser/gems/cache"
    /opt/chef/embedded/bin/gem install busser --no-rdoc --no-ri
    /opt/chef/embedded/bin/gem install serverspec --no-rdoc --no-ri
    /opt/busser/gems/bin/busser setup
    /opt/busser/gems/bin/busser plugin install busser-serverspec

Vagrantfiles for Ubuntu 12 and 14 are provided. Once they're built, export them with `vagrant package`.

You can then add those images to vagrant with the same name as the ones they're replacing or use this syntax to load them instead:

    platforms:
      - name: ubuntu-12.04
        driver:
            box: tk-12
      - name: ubuntu-14.04
        driver:
            box: tk-14
