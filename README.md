# WordPress Staging to Production URL Replacer

This set of functions for WordPress allows you to seamlessly replace staging URLs with production URLs for attachments and their image attributes. This is particularly useful when working on a local development environment or a staging server, preventing the unnecessary download of all images uploaded in the `wp-content/uploads` directory.

## Usage

1. Open your `wp-config.php` file.

2. Add the following code at the end of the file, just before the closing PHP tag (`?>`), if it exists.

   ```bash
   // Define localhost and production URLs
   define('LOCALHOST_URL', 'http://xxxxxxx.localhost/');
   define('PRODUCTION_URL', 'http://xxxxxxx.com/');

   /**
    * Filter to replace attachment URL in staging environment
    *
    * @param string $url        The attachment URL.
    * @param object $attachment The attachment object.
    *
    * @return string The modified attachment URL.
    */
   add_filter('wp_get_attachment_url', 'xplodman_staging_wp_get_attachment_url', 1000000, 2);
   function xplodman_staging_wp_get_attachment_url($url, $attachment)
   {
       // Check if the URL contains the localhost URL
       if (strpos($url, LOCALHOST_URL) !== false) {
           // Replace localhost URL with production URL
           $url = str_replace(LOCALHOST_URL, PRODUCTION_URL, $url);
       }
       return $url;
   }

   /**
    * Filter to replace attachment image attributes in staging environment
    *
    * @param array $attributes The attachment image attributes.
    *
    * @return array The modified attachment image attributes.
    */
   add_filter('wp_get_attachment_image_attributes', 'xplodman_staging_wp_get_attachment_image_attributes');
   function xplodman_staging_wp_get_attachment_image_attributes($attributes)
   {
       // Check and replace 'src' attribute if it contains localhost URL
       if (isset($attributes['src']) && strpos($attributes['src'], LOCALHOST_URL) !== false) {
           $attributes['src'] = str_replace(LOCALHOST_URL, PRODUCTION_URL, $attributes['src']);
       }

       // Check and replace 'srcset' attribute if it contains localhost URL
       if (isset($attributes['srcset']) && strpos($attributes['srcset'], LOCALHOST_URL) !== false) {
           $attributes['srcset'] = str_replace(LOCALHOST_URL, PRODUCTION_URL, $attributes['srcset']);
       }

       return $attributes;
   }
   ```
3. Save the wp-config.php file.

Enjoy hassle-free URL replacement when working on staging environments, and avoid downloading all images uploaded in the `wp-content/uploads` directory!

Feel free to customize the production URL and adapt the code to fit your specific needs.

## License

This code is licensed under the MIT License.
