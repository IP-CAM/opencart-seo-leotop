Front-end links

The best way to make a front-end link from the Admin area without modifying core files is to create your own URL object:

    $url = new Url(HTTP_CATALOG, $this->config->get('config_secure') ? HTTP_CATALOG : HTTPS_CATALOG);

To create URLs you then use $url instead of $this->url, so your example would be:

    $url->link('product/product', 'product_id=' . $row['product_id']);
  
SEO URLs

Getting SEO URLs is a bit harder and unfortunately requires copying some code or a bit of bodging.
Copied Code

Basically, copy the entire rewrite($link) function from catalog/controller/common/seo_url.php and add it to the controller class that you're creating. Then add the following line after the new $url line mentioned above:

    $url->addRewrite($this);

The current controller is then used as a rewriter and all URLs get rewritten. You might be able to use a separate class, but then there are dependencies to get the database access. This seemed the most direct way of doing it, even if it is ugly.

Your code should then be:
    
    <?php
    class ControllerMyController extends Controller {
      public function index() {
        ...
        $url = new Url(HTTP_CATALOG, $this->config->get('config_secure') ? HTTP_CATALOG : HTTPS_CATALOG);
        if ($this->config->get('config_seo_url')) {
          $url->addRewrite($this);
        }
        $url->link('product/product', 'product_id=' . $row['product_id']);
        ...
      }
    
      public function rewrite($link) {
        ... [stuff from seo_url.php] ...
      }
    }
    
and you'll get SEO'd URLs from the admin area.
Bodging

If you're happy with an arbitrary require statement, then you can alternatively do the following and use the existing SEO code (which means that it'll stay up-to-date if it changes, but will fail if the file moves).

To create SEO URLs this way your code needs to be:

    $url = new Url(HTTP_CATALOG, $this->config->get('config_secure') ? HTTP_CATALOG : HTTPS_CATALOG);
                if ($this->config->get('config_seo_url')) {
                        // Require the SEO file directly - path is relative to /admin/index.php
                        require_once('../catalog/controller/common/seo_url.php');
                        $rewriter = new ControllerCommonSeoUrl($this->registry);
                        $url->addRewrite($rewriter);
                }
                $url->link('product/product', 'product_id=' . $row['product_id']);
