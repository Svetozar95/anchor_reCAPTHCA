1) Create a folder anchor / plugins / reCAPTCHA /

2) Download the file to a folder  anchor / plugins / reCAPTCHA / recaptchalib.php

3) Edit the file according to the file /anchor/routes/site.php site.php




- I turned off the check messages for spam.
- In my code are sent a notification email when any communication
--anchor / plugins / reCAPTCHA / recaptchalib.php taken from free sources. Later, lay a link to the original




--------
changed code:

// i dissabled check is possibly spam


	// check if the comment is possibly spam
//	if($spam = Comment::spam($input)) {
//		$input['status'] = 'spam';
//	}


// reCAPTHA 2.0

require_once "anchor/plugins/reCAPTCHA/recaptchalib.php";
// Register API keys at https://www.google.com/recaptcha/admin
$siteKey = "6LdRrv8SAAAAANuGJ-NSrgMQiOOdycqWLvqAmz12";
$secret = "6LdRrv8SAAAAAPMwQfo3fwKi0Mwklf9TKSVAQcgX";
// reCAPTCHA supported 40+ languages listed here: https://developers.google.com/recaptcha/docs/language
$lang = "ru";
// The response from reCAPTCHA
$resp = null;
// The error code from reCAPTCHA, if any
$error = null;
$reCaptcha = new ReCaptcha($secret);
// Was there a reCAPTCHA response?
if ($_POST["g-recaptcha-response"]) {
$resp = $reCaptcha->verifyResponse($_SERVER["REMOTE_ADDR"],$_POST["g-recaptcha-response"]);
}


if ($resp != null && $resp->success) {

// test echo;
//echo "комментарий добавлен";


	$comment = Comment::create($input);

	Notify::success(__('comments.created'));

	// dont notify if we have marked as spam
//	if( ! $spam and Config::meta('comment_notifications')) {


      if(! Config::meta('comment_notifications')) {

		$comment->notify();
	}
	return Response::redirect($posts_page->slug . '/' . $slug . '#comment');

} else {


echo "error adding comments. you robot!!";

        return Response::redirect($posts_page->slug . '/' . $slug . '#comment');



}

});
