---
$title: Structure and rendering of AMP emails
order: 2
formats:
  - email
teaser:
  text: >-
    Email is structured as a MIME tree. This MIME tree contains the message body
    and any attachments to the email.
toc: true
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/email/amp-email-structure.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2018 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->



Email is structured as a [MIME tree](https://en.wikipedia.org/wiki/MIME). This MIME tree contains the message body and any attachments to the email.

To embed AMP within an email, add a new MIME part with a content type of `text/x-amp-html` as a descendant of `multipart/alternative`. It should live alongside the existing `text/html` or `text/plain` parts. This ensures that the email message works on all clients.

<amp-img alt="AMP for Email MIME Parts Diagram"
    layout="responsive"
    width="752" height="246"
    src="https://github.com/ampproject/amphtml/raw/master/spec/img/amp-email-mime-parts.png">
<noscript>
<img alt="AMP for Email MIME Parts Diagram" src="../img/amp-email-mime-parts.png" />
</noscript>
</amp-img>

For more information about the `multipart/alternative` subtype, refer to [RFC 1521, section 7.2.3](https://tools.ietf.org/html/rfc1521#section-7.2.3).

## Additional information <a name="additional-information"></a>

The `text/x-amp-html` part must be nested under a `multipart/alternative` node.
An email cannot have more than one `text/x-amp-html` part inside a `multipart/alternative` node.

The `multipart/alternative` must contain at least one non-AMP (`text/plain` or `text/html`) node in addition to the
`text/x-amp-html` node. This will be displayed to users whose email clients don't support AMP or who opted out via
their email provider's settings.

Note: Some email clients[[1]](https://openradar.appspot.com/radar?id=6054696888303616) will only render the last MIME part,
so we recommend placing the `text/x-amp-html` MIME part _before_ the `text/html` MIME part.

### Replying/forwarding semantics <a name="replyingforwarding-semantics"></a>

The email client strips out the `text/x-amp-html` part of the MIME tree when a user replies to or forwards an AMP email message.

### Expiry <a name="expiry"></a>

The email client may stop displaying the AMP part of an email after a set period of time, e.g. 30 days. In this
case, emails will display the `text/html` or `text/plain` part.

## Example <a name="example"></a>

<!-- prettier-ignore-start -->
[sourcecode:html]
From:  Person A <persona@example.com>
To: Person B <personb@example.com>
Subject: An AMP email!
Content-Type: multipart/alternative; boundary="001a114634ac3555ae05525685ae"

--001a114634ac3555ae05525685ae
Content-Type: text/plain; charset="UTF-8"; format=flowed; delsp=yes

Hello World in plain text!

--001a114634ac3555ae05525685ae
Content-Type: text/x-amp-html; charset="UTF-8"

<!doctype html>
<html ⚡4email>
<head>
  <meta charset="utf-8">
  <style amp4email-boilerplate>body{visibility:hidden}</style>
  <script async src="https://cdn.ampproject.org/v0.js"></script>
</head>
<body>
Hello World in AMP!
</body>
</html>
--001a114634ac3555ae05525685ae
Content-Type: text/html; charset="UTF-8"

<span>Hello World in HTML!</span>
--001a114634ac3555ae05525685ae--
[/sourcecode]
<!-- prettier-ignore-end -->


"<div data-md-type=\"front_matter\" class=\"front_matter\">\n<table>\n<tr>\n<td class=\"locki-notrack\">$title</td>\n<td data-segment-id=\"12596199\">Structure and rendering of AMP emails</td>\n</tr>\n<tr>\n<td class=\"locki-notrack\">order</td>\n<td data-segment-id=\"12596200\">2</td>\n</tr>\n<tr>\n<td class=\"locki-notrack\">formats</td>\n<td>\n<ul>\n<li data-segment-id=\"12596202\">email</li>\n</ul>\n</td>\n</tr>\n<tr>\n<td class=\"locki-notrack\">teaser</td>\n<td>\n<table>\n<tr>\n<td class=\"locki-notrack\">text</td>\n<td data-segment-id=\"12596203\">\nEmail is structured as a MIME tree. This MIME tree contains the message body\nand any attachments to the email.</td>\n</tr>\n</table>\n</td>\n</tr>\n<tr>\n<td class=\"locki-notrack\">toc</td>\n<td data-segment-id=\"12596201\">true</td>\n</tr>\n</table>\n</div>\n\n<div data-md-type=\"block_html\">\n<!--\nThis file is imported from https://github.com/ampproject/amphtml/blob/master/spec/email/amp-email-structure.md.\nPlease do not change this file.\nIf you have found a bug or an issue please\nhave a look and request a pull request there.\n-->\n</div>\n<div data-md-type=\"block_html\">\n<!---\nCopyright 2018 The AMP HTML Authors. All Rights Reserved.\n\nLicensed under the Apache License, Version 2.0 (the \"License\");\nyou may not use this file except in compliance with the License.\nYou may obtain a copy of the License at\n\n      http://www.apache.org/licenses/LICENSE-2.0\n\nUnless required by applicable law or agreed to in writing, software\ndistributed under the License is distributed on an \"AS-IS\" BASIS,\nWITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.\nSee the License for the specific language governing permissions and\nlimitations under the License.\n-->\n</div>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596185\">Email is structured as a <a href=\"https://en.wikipedia.org/wiki/MIME\" data-md-type=\"link\">MIME tree</a>. This MIME tree contains the message body and any attachments to the email.</p>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596186\">To embed AMP within an email, add a new MIME part with a content type of <code data-md-type=\"codespan\">text/x-amp-html</code> as a descendant of <code data-md-type=\"codespan\">multipart/alternative</code>. It should live alongside the existing <code data-md-type=\"codespan\">text/html</code> or <code data-md-type=\"codespan\">text/plain</code> parts. This ensures that the email message works on all clients.</p>\n<p data-md-type=\"paragraph\"><amp-img alt=\"AMP for Email MIME Parts Diagram\" layout=\"responsive\" width=\"752\" height=\"246\" src=\"https://github.com/ampproject/amphtml/raw/master/spec/img/amp-email-mime-parts.png\">\n<noscript data-md-type=\"raw_html\" data-segment-id=\"12596198\"> <img data-md-type=\"raw_html\" alt=\"AMP for Email MIME Parts Diagram\" src=\"../img/amp-email-mime-parts.png\"> </noscript>\n</amp-img></p>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596187\">For more information about the <code data-md-type=\"codespan\">multipart/alternative</code> subtype, refer to <a href=\"https://tools.ietf.org/html/rfc1521#section-7.2.3\" data-md-type=\"link\">RFC 1521, section 7.2.3</a>.</p>\n<h2 data-md-type=\"header\" data-md-header-level=\"2\" data-segment-id=\"12596188\">Additional
information <a data-md-type=\"raw_html\" name=\"additional-information\"></a>\n</h2>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596189\">The <code data-md-type=\"codespan\">text/x-amp-html</code> part must be nested under a <code data-md-type=\"codespan\">multipart/alternative</code> node. An email cannot have more than one <code data-md-type=\"codespan\">text/x-amp-html</code> part inside a <code data-md-type=\"codespan\">multipart/alternative</code> node.</p>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596190\">The <code data-md-type=\"codespan\">multipart/alternative</code> must contain at least one non-AMP (<code data-md-type=\"codespan\">text/plain</code> or <code data-md-type=\"codespan\">text/html</code>) node in addition to the <code data-md-type=\"codespan\">text/x-amp-html</code> node. This will be displayed to users whose email clients don't support AMP or who opted out via their email provider's settings.</p>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596191\">Note: Some email clients<a href=\"https://openradar.appspot.com/radar?id=6054696888303616\" data-md-type=\"link\">[1]</a> will only render the last MIME part, so we recommend
placing the <code data-md-type=\"codespan\">text/x-amp-html</code> MIME part <em data-md-type=\"emphasis\">before</em> the <code data-md-type=\"codespan\">text/html</code> MIME part.</p>\n<h3 data-md-type=\"header\" data-md-header-level=\"3\" data-segment-id=\"12596192\">Replying/forwarding semantics <a data-md-type=\"raw_html\" name=\"replyingforwarding-semantics\"></a>\n</h3>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596193\">The email client strips out the <code data-md-type=\"codespan\">text/x-amp-html</code> part of the MIME tree when a user replies to or forwards an AMP email message.</p>\n<h3 data-md-type=\"header\" data-md-header-level=\"3\" data-segment-id=\"12596194\">Expiry <a data-md-type=\"raw_html\" name=\"expiry\"></a>\n</h3>\n<p data-md-type=\"paragraph\" data-segment-id=\"12596195\">The email client may stop displaying the AMP part of an email after a set period of time, e.g. 30 days. In this case, emails will display the <code data-md-type=\"codespan\">text/html</code> or <code data-md-type=\"codespan\">text/plain</code> part.</p>\n<h2 data-md-type=\"header\" data-md-header-level=\"2\" data-segment-id=\"12596196\">Example <a data-md-type=\"raw_html\" name=\"example\"></a>\n</h2>\n<div data-md-type=\"block_html\">\n<!-- prettier-ignore-start -->\n</div>\n<pre data-md-type=\"custom_block_code\"><code data-segment-id=\"12596197\">[sourcecode:html]\nFrom:  Person A &lt;persona@example.com&gt;\nTo: Person B &lt;personb@example.com&gt;\nSubject: An AMP email!\nContent-Type: multipart/alternative; boundary=\"001a114634ac3555ae05525685ae\"\n\n--001a114634ac3555ae05525685ae\nContent-Type: text/plain; charset=\"UTF-8\"; format=flowed; delsp=yes\n\nHello World in plain text!\n\n--001a114634ac3555ae05525685ae\nContent-Type: text/x-amp-html; charset=\"UTF-8\"\n\n&lt;!doctype html&gt;\n&lt;html ⚡4email&gt;\n&lt;head&gt;\n  &lt;meta charset=\"utf-8\"&g
t;\n  &lt;style amp4email-boilerplate&gt;body{visibility:hidden}&lt;/style&gt;\n  &lt;script async src=\"https://cdn.ampproject.org/v0.js\"&gt;&lt;/script&gt;\n&lt;/head&gt;\n&lt;body&gt;\nHello World in AMP!\n&lt;/body&gt;\n&lt;/html&gt;\n--001a114634ac3555ae05525685ae\nContent-Type: text/html; charset=\"UTF-8\"\n\n&lt;span&gt;Hello World in HTML!&lt;/span&gt;\n--001a114634ac3555ae05525685ae--\n[/sourcecode]</code></pre>\n<div data-md-type=\"block_html\">\n<!-- prettier-ignore-end -->\n</div>\n"
