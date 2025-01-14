# محدودیت تعداد خرید محصول در ووکامرس (WooCommerce Limit Product Purchase Quantity)

This guide explains how to limit the maximum purchase quantity of a specific product in WooCommerce. This code ensures users cannot purchase more than the defined quantity of a product.

## Features
- Limit the number of a specific product each user can purchase.
- Display an error message when exceeding the limit.
- Simple and customizable.

## Requirements
1. A WordPress website with WooCommerce active.
2. Access to the `functions.php` file of the active theme or a custom plugin.

## Implementation

Add the following code to your theme's `functions.php` file or a custom plugin:

```php
add_action('woocommerce_add_to_cart_validation', 'limit_product_quantity_in_cart', 10, 3);

function limit_product_quantity_in_cart($passed, $product_id, $quantity) {
    // Target product ID
    $target_product_id = 123; // Replace 123 with your product ID
    $max_quantity = 2; // Maximum allowed quantity

    // Check if the target product is in the cart
    foreach (WC()->cart->get_cart() as $cart_item) {
        if ($cart_item['product_id'] == $target_product_id) {
            $current_quantity = $cart_item['quantity'];
            $new_quantity = $current_quantity + $quantity;

            if ($new_quantity > $max_quantity) {
                wc_add_notice(
                    sprintf(
                        'You cannot purchase more than %d of this product.',
                        $max_quantity
                    ), 
                    'error'
                );
                return false;
            }
        }
    }

    return $passed;
}
```

```php
add_action('woocommerce_add_to_cart_validation', 'limit_product_quantity_in_cart', 10, 3);

function limit_product_quantity_in_cart($passed, $product_id, $quantity) {
    // ID محصول مورد نظر
    $target_product_id = 123; // به جای 123، آی‌دی محصول مورد نظر را وارد کنید
    $max_quantity = 2; // حداکثر تعداد مجاز

    // بررسی اینکه آیا محصول مورد نظر در سبد خرید وجود دارد
    foreach (WC()->cart->get_cart() as $cart_item) {
        if ($cart_item['product_id'] == $target_product_id) {
            $current_quantity = $cart_item['quantity'];
            $new_quantity = $current_quantity + $quantity;

            if ($new_quantity > $max_quantity) {
                wc_add_notice(
                    sprintf(
                        'شما نمی‌توانید بیش از %d عدد از این محصول خریداری کنید.',
                        $max_quantity
                    ), 
                    'error'
                );
                return false;
            }
        }
    }

    return $passed;
}
```

## How It Works
1. **Target Product ID**: Set the product ID in the `$target_product_id` variable.
2. **Maximum Quantity**: Define the allowed purchase limit in the `$max_quantity` variable.
3. **Cart Check**: The code checks the cart and, if the new quantity exceeds the limit, displays an error message and prevents adding the product.

## یافتن آیدی محصول
1. به پیشخوان وردپرس بروید.
2. به **محصولات > همه محصولات** بروید.
3. نشانگر ماوس را روی محصول مورد نظر ببرید و آیدی نمایش داده شده زیر نام محصول را یادداشت کنید.

## Example Error Message
When attempting to add more than the allowed quantity, the following error message will be displayed:

```
شما نمی‌توانید بیش از 2 عدد از این محصول خریداری کنید.
```

```
You cannot purchase more than 2 of this product.
```

## Customization
### Change Error Message
You can modify the error message in the `wc_add_notice` function. For example:

```php
wc_add_notice('You cannot purchase more than ' . $max_quantity . ' of this product.', 'error');
```

### Apply Limit to Multiple Products
To apply the limit to multiple products, use an array of product IDs:

```php
$target_product_ids = [123, 456, 789];
if (in_array($cart_item['product_id'], $target_product_ids)) {
    // Apply the same logic
}
```

## Notes
- Ensure this code does not conflict with other WooCommerce customizations.
- Test thoroughly before deploying on a live website.

## License / مجوز
This code is provided under the MIT License. Feel free to use and modify it as needed.

این کد تحت مجوز MIT ارائه می‌شود. می‌توانید از آن استفاده کرده و به دلخواه تغییر دهید.

---

برای سوالات یا همکاری، لطفاً یک issue یا pull request باز کنید. 😊
