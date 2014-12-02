SEO PATCH

file: /catalog/controller/product/category.php
line number: 249
original line: 'href'        => $this->url->link('product/product', 'path=' . $this->request->get['path'] . '&product_id=' . $result['product_id'] . $url),
change line  : 'href'        => $this->url->link('product/product', 'product_id=' . $result['product_id'] . $url),
