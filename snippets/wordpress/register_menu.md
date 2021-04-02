# Register Menu in WordPress


### 注册菜单
```php
function custom_menus() {
 register_nav_menus(
  array(
   'primary' => 'Primary menu',
   'secondary' => 'Secondary menu',
   'tertiary' => 'Tertiary menu'
  )
 );
}

add_action( 'init', 'custom_menus' );
```

### 在前端使用菜单

```php
wp_nav_menu(
 array(
  'theme_location' => 'primary',
  'container' => false,
 )
);

wp_nav_menu(
 array(
  'menu' => 'primary',
  'container' => false,
 )
);
```
