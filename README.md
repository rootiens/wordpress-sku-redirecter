# wordpress-sku-redirecter

### DB Query
```sql
SELECT wp_postmeta.meta_value as sku,
       wp_posts.ID as product_id
FROM wp_posts
INNER JOIN wp_postmeta 
       ON wp_posts.ID = wp_postmeta.post_id
WHERE wp_posts.post_type LIKE 'product%'
AND wp_postmeta.meta_key = '_sku'
AND wp_postmeta.meta_value = {sku}
```
### Func

```php
add_action('template_redirect', 'sku_product_redirect');
function sku_product_redirect() {
    $sku = get_query_var('product');
    
    if ( ! empty( $sku ) ) {
        $product_id = (int) wc_get_product_id_by_sku( $sku );
        
        if( $product_id > 0 ) {
            wp_safe_redirect( get_permalink($product_id) );
            exit;
        }
    }
}
```
