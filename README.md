# Docs and samples for how to manipulate centra API values with vouchers

This repository shows how Centra response can be manipulated to show voucher the same way for both - payment-result (receipt) responses and selection responses.  

At the moment the data you have to work with is fairly raw but we plan to provide some aggregations of this data to make this operation easier in the near future, when these ways are available we will provide documentation about it on [docs.centra.com](https://docs.centra.com/)

# Why is this needed ?
Traditionally Centra has shown the voucher discount on `order` level for selections, no matter the voucher setting. This was done with intention to be able nicely show the customer how much in total they have saved.

But for an order, returns become a lot easier to handle if voucher is distributed to items, so when a selection turns into an order (by completing the checkout) the sum form before is now moved to items.

This is a behaviour we can not just change for our APIs so to get a different behaviour we need to introduce new ways of doing things.

# How can I get it to show the same way ?
With recent changes we are now providing much more detailed data for vouchers, so it is now possible to manipulate the totals Centra gives to be able to give the correct value before or after voucher on items, and also per order. Best way to understand how to do it is to look at the [voucher_manipulation_docs_and_playground.html](voucher_manipulation_docs_and_playground.html) file, but we will try to give a short text explanation as well. Note that more and easier ways to achieve these results will come in the near future.

Each item now has a list of vouchers which has modified its price, exactly how much in total a line has been modified per voucher can be found in the `items.discounts` object, the output format mostly matches the order level discounts object. This object also includes a boolean `hasAffectedItemPrice` which tells you if the items `totalPrice` has already been reduced by the listed voucher. This value is always false for `selections` and `true` on order for any voucher setup to be on `order items`. So now `all` that needs to be done is to correctly sum things up and modify the normal values provided under `items` and `discounts` to get the wanted result. See [voucher_manipulation_docs_and_playground.html](voucher_manipulation_docs_and_playground.html)l for more details.

# How can i use this repository ?
This repository consists of a html file and some sampled json responses as they are given by different Centra APIs, the responses have been simplified and are based on anonymous test data.

The source of the [voucher_manipulation_docs_and_playground.html](voucher_manipulation_docs_and_playground.html) file contains some JS functions with comments explaining what is being done and why. The functions show how the response data can be used to show voucher data the same way no matter if it is from a selection or order response, it is primarily setup to show voucher as if on "order" level, but also hints how to always show on "item" level.

The file itself can be run (download and open in a browser or use github.io). When opened it shows a textarea where you can paste a response from centra (both Shop and Checkout API responses should work), use the json files in repository to test with, or responses from your own Centra instance. The file will then generate a order view based on that response.

Note that this code is written as a learning tool, use it to start your own experimentation but it should not be considered production ready. 
