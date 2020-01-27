# Changes in Commerce 3

Upgrading Commerce 2 to Commerce 3 is a smaller task than the previous Commerce 1 to Commerce 2 udgrade. 

If you are coming from Commerce 1, migrating to Commerce 2 first would be the bulk of the work. Things that were deprecated in Commerce 2, are now breaking changes in Commerce 3.

All changes are documented in the [full changelog](https://github.com/craftcms/commerce/blob/master/CHANGELOG.md#300---2020-01-28). 

The information below is for additional context and to make the transition easier.


## Order Emails

Order notification emails are now sent via a queue job, so running a [queue worker as a daemon](https://nystudio107.com/blog/robust-queue-job-handling-in-craft-cms)  is highly recommended to avoid customer email notification delays.

Previously emails would be generated during customer checkout, which could cause the order completion page to take a prolonged time to display (especially with PDF generation involved). This change gives your customers a better checkout experience.

No changes are needed for emails to continue to work, but ensuring your queue is working correctly will ensure everything goes smoothly.


## Edit Order page

Plugins and modules that modify the Edit Order page are likely to break with this update as the page is now a [Vue.js](https://vuejs.org/) application. The same Twig template hooksx are still available, but inserting into the part of the DOM controlled by Vue.js will not work.


## Data tables

All data tables througout the control panel use the new Craft 3.4 Vue.js-based data table, so any extensions of those old HTML tables are likely to break.


## Permissions

We have added the “Edit orders” and “Delete orders” user permissions, but users with the existing “Manage orders” permission will not automatically get these new permissions, so updating those users and user groups would be required.


## Cart merging

The cart is still retrieved from the front-end templatess using`craft.commerce.carts.cart`in your templates, but the optional `mergeCarts` param has been removed, and it is no longer possible to automicatically merge previous carts of the current user. 

We now recommend the customer manually adds items from the [previous carts to their current cart](adding-to-and-updating-the-cart.md#restoring-previous-cart-contents). An example of this is in the docs.

Merging carts as manual process is better since the customer can decide what to do with any validation issues like maximum quanity rules. The previous merge feature would just fail to merge with no error messages. 

This change is also mitigated by the fact that the previous cart of the current user is now loaded as the current cart when calling `craft.commerce.carts.cart` automatically.


## Twig template changes


Use the table below to update your twig templates.
D - Deprecated
BC - Breaking Change

| Old                                       | New                                                         | Change
|-------------------------------------------|-------------------------------------------------------------|--------
| `craft.commerce.carts.cart(true, true)  ` | `craft.commerce.carts.cart(true)`                           | BC
| `craft.commerce.carts.cart(false, true)   | `craft.commerce.carts.cart(false)`                          | BC
| `craft.commerce.availableShippingMethods` | `cart.availableShippingMethod`                              | BC
| `craft.commerce.cart`                     | `craft.commerce.carts.cart`                                 | BC
| `craft.commerce.countriesList`            | `craft.commerce.countries.allStatesAsListGroupedByCountryId`| BC
| `craft.commerce.customer`                 | `craft.commerce.customers.customer`                         | BC
| `craft.commerce.discountByCode`           | `craft.commerce.discounts.discountByCode`                   | BC
| `craft.commerce.primaryPaymentCurrency`   | `craft.commerce.paymentCurrencies.primaryPaymentCurrency`   | BC
| `craft.commerce.statesArray`              | `craft.commerce.states.allStatesAsList`                     | BC



### Form Action Changes

| Old                                       | New                            | Docs
|-------------------------------------------|--------------------------------|-------
| `commerce/cart/remove-line-item`          | `commerce/cart/update-cart`    | [Updating the Cart](adding-to-and-updating-the-cart.md#updating-line-items)
| `commerce/cart/update-line-item`          | `commerce/cart/update-cart`    | [Updating the Cart](adding-to-and-updating-the-cart.md#updating-line-items)
| `commerce/cart/remove-all-line-items`     | `commerce/cart/update-cart`    | [Updating the Cart](adding-to-and-updating-the-cart.md#updating-line-items)