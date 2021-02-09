Sending a message to a user requires a unique recipient identifier. There are three types of identifiers that are supported by the `Send API`.

The first is the `id`-identifier. You can get the recipient `id` as soon as a user sends a message to your bot. Please be aware of the fact that the `recipient id` is *not* the user id. If you try to send a message to a `user id` this will not work. You are required to use the `recipient id`.

If you received a message and took the `id` from that message you can create a new `IdMessageRecipient` with this simple line of code:

{% highlight java %}
IdMessageRecipient recipient = new IdMessageRecipient("<userID>");
{% endhighlight %}

The second identifier type is the phone number of the user you are attempting to contact. Facebook only sends a message to a phone number if there is a certain level of confidence the the phone number is reachable. Additionally, sending messages to a phone number recipient requires the customer matching feature to be enabled for the app.

<div class="rfb-callout info" role="alert">
	<h4>
		Customer Matching requirements and availability
	</h4>
	<p>
		Only bots that belong to a Facebook page with a U.S. based administrator can use the <a href="https://developers.facebook.com/docs/messenger-platform/identity/customer-matching/" target="_blank">customer matching</a> feature. Phone numbers may belong to users in other countries, too.
	</p>
	<p>
		You can only send messages as long as the user has not previously opted-out. As soon as she receives the message, a message request is shown and then she can read the message. Afterwards she can decide whether to accept or decline further messages. If the message request is accepted, the bot is allowed to send further messages.
	</p>
</div>

With this small piece of code you can create a phone recipient:

{% highlight java %}
PhoneMessageRecipient recipient = new PhoneMessageRecipient("<userPhone>");
{% endhighlight %}

And last, but not least, there is the `user_ref`. This reference is fetched from a webhook message and used to create a `UserRefMessageRecipient` object. The `user_ref` is send to the webhook as soon as the <a target="_blank" href="https://developers.facebook.com/docs/messenger-platform/discovery/checkbox-plugin">Facebook Checkbox Plugin</a> is used and a user opts-in to receive messages from the website that shows this plugin. 

The code to create a `UserRefMessageRecipient` is:

{% highlight java %}
UserRefMessageRecipient recipient = new UserRefMessageRecipient("<userRef>");
{% endhighlight %}

### Private Replies and the Messenger API

Private replies provide a way to send private messages to a user who created a post or a comment on a Facebook page. So you can start a private conversation from a public "entry point".

Since <a href="_blank" href="https://developers.facebook.com/docs/messenger-platform/discovery/private-replies">September 30, 2019</a> Facebook allows to use the Send API for private replies. This leads to much better way to communicate with a user and you can use all the cool features the Send API provides.

To use this new way to start the conversation, you simply need the `post` or `comment` id. With this id you create a recipient object like this:

{% highlight java %}
// private reply recipient based on a post
String postId = post.getId(); 
PostIdMessageRecipient recipient = new PostIdMessageRecipient(postId);

// private reply recipient based on a comment
String commentId = comment.getId();
CommentIdMessageRecipient recipient = new CommentIdMessageRecipient(commentId);
{% endhighlight %}

Then you can use this object as recipient and you are done.