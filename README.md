# Initialize a drupal11 recipe
mkdir backend \
  && cd backend \
  && lando init \
    --source cwd \
    --recipe drupal11 \
    --webroot web \
    --name backend

# Start the environment
lando start
    
# Create latest drupal11 project via composer
lando composer create-project drupal/recommended-project:11.x tmp && cp -r tmp/. . && rm -rf tmp

# Composer can timeout on install for some machines, if that happens, run the following command and then re-run the previous lando composer command:
# lando composer config --global process-timeout 2000

# Install a site local drush
lando composer require drush/drush

# Install drupal
lando drush site:install --db-url=mysql://drupal11:drupal11@database/drupal11 -y

# List information about this app
lando info