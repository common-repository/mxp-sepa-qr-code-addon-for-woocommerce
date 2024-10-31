=== SEPA QR-Code for Woocommerce (GDPR-compliant) ===
Contributors: thedoctorcoernel
Tags: WooCommerce, Payment, QR-Code, Bacs, Sepa-QR
Requires at least: 4.7
Tested up to: 6.2
Stable tag: 1.1.0
Requires PHP: 7.0
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html

Adds a SEPA-QR Code for bank transfer payments (bacs) in the WooCommerce Thankyou page and Woocommer emails. The QR-Code can be hooked into other plugins.

=== Before you start ===
The plugin comes as is and free. However a real person has put real work into it. So if you use it please do s.th. good. Use your efforts, your time for beneficial projects or whatever!
=== Prerequisite ===
php GD2 extension must be installed as the QR-Code generator by [fellwell15](https://github.com/fellwell5/bezahlcode/) requires this.
=== Installation ===
Nothing special:
* Click install and activate
== Hooking into other plugins ==
I use a plugin for [PDF-invoices and packaging slips](https://docs.wpovernight.com/home/woocommerce-pdf-invoices-packing-slips/pdf-template-action-hooks/).  Refer to this sample to hook the QR-Code into whatever you like:

`/wp-content/themes/Your(Child)Theme/functions.php`

`
/* QR-Code in invoices */
add_action( 'wpo_wcpdf_after_order_details', 'wpo_wcpdf_qr_code', 10, 2 );
function wpo_wcpdf_qr_code ($document_type, $order) {
	require_once WP_PLUGIN_DIR . '/mxp-sepa-qr-code-addon-for-woocommerce/muxp-sepaqr.php';
    $muxp_order = wc_get_order( $order);
	$order_id  = $order->get_id();
 	if ( !empty($muxp_order->get_total()) && (float)$order->get_total() > 0 ) {
		echo '<h1>QR-Code for your online banking app<h1>';
		echo '<img class="muxp-bacs-qrcode" src="' . esc_attr(muxp_get_qrcode($order->get_total(), $order_id)) . '" alt="qr-code"></p>';
	} 
}
`

== What happens in the backend: ==
* the QR code generator creates the QR-code locally. There is **no Google-API nor external server in use**!
* the QR code generator is from [fellwell15](https://github.com/fellwell5/bezahlcode/)
* plugin registers a get-parameter (configurable, default mxp_qr) for testing purposes and, if desired, to create links to the cached QR codes.
* the prefix mxp is used throghout the plugin to avoid collisions with other plugins and functions. mxp stands for www.musicalexperten.de (musical experts). Remember where you've seen it first! ;-)

=== Testing and troubleshooting ===
== Simple way ==
Install the plugin and order s.th. in your shop using BACS (direct bank transfer).
== To test if the QR-Code generator is working ==
www.yourwebpage.de/?mxp_qr=something  = creates a real QR with dummyvalues 11-11
[Working example](https://www.musicalexperten.de/?mxp_qr=something)
== To find an existing cached QR-Code, query for a valid md5 string == If it does not exist in cache or transients, a sad smiley will appear.
www.yourwebpage.de/?mxp_qr=351436ef4b279e1811a6c68a2dd58b1b 
results in a sad smiley. [Working example](https://www.musicalexperten.de/?mxp_qr=351436ef4b279e1811a6c68a2dd58b1b)

== Remarks ==
Storing the QR code in cache or transients is only needed if you want to use a link instead of a picture inside the email. Details in the program code.

== Support ==
The program has been written by a professional programmer - however fully free of charge and without detailed knowledge about WooCommerce. I try to assist you in the support forum or on GitHub the best I can but my knowledge is limited.

== Full integration in Woocommerce ==
I am more then happy if someone integrates the code into the Woocommerce core! The topic is discussed here: https://github.com/woocommerce/woocommerce/issues/27661


== Frequently Asked Questions ==

= I can't see the QR-Code in a specific email client =

This surely is due to the fact that some email clients won't show Base64-encoded images. Your help is appreciated! Have a look at https://github.com/Coernel82/SEPA-QR-for-Woocommerce/issues/17
Workaround: Hook it into your PDF-invoices! (see above!)

= What about privacy / GDPR =

The plugin creates the QR-Code on your server and it does not use any external resources.


== Screenshots ==

1. the QR-Code gets added to the WooCommerce order email
2. example how the qr-code is hooked into a pdf envoice

== Changelog ==

= 1.1.0 =
* added loacalization
* added prefix muxp to the bacs-qrcode css class for the QR code. If you have used .bacs-qrcode you have to change it to muxp-bacs-qrcode

= 1.0.4 =
* initial commit to the wordpress directory

== Upgrade Notice ==

= 1.1.0 =
* added loacalization

= 1.0.4 =
* initial commit to the wordpress directory