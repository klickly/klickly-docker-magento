# klickly-docker-magento

This package is fork [markshust/docker-magento](https://github.com/markshust/docker-magento).

### Manual to set up Magento 2:

1. Open terminal
2. Run ```curl -s https://raw.githubusercontent.com/klickly/klickly-docker-magento/master/lib/onelinesetup|bash -s -- magento2-shop-1.test 2.3.0 true```

Where:
  - `magento2-shop-1.test` - hostname (should be unique)
  - `2.3.0` - version of Magento
  - `true` - flag to install Magento with klickly extension by default

After installation in console will be message with uri to access created shop. Admin panel will be available at /admin. Credentials to log in:
  - Username: `admin`
  - Password: `admin123`

Shop sources will be placed at ~/Magento-Shops/{hostname}