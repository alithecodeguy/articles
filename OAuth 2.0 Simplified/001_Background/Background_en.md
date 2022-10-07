# Preface

##### _by Aaron Parecki_

I first got involved with OAuth in 2010 when I was building an API, and knew that I wanted third-party developers to be able to build apps on top of it. At the time, OAuth seemed incredibly intimidating. There were only a few implementations of OAuth 1 in existence, and OAuth 2.0 was still a rough draft. One night I decided to sit down with a craft beer and a paper copy of the latest draft and read it from start to finish until I understood it.

After wading through the forty-four-page spec, I learned a couple things: reading specs is not the best way to learn how OAuth works, and OAuth 2.0 wasn’t nearly as complicated as I originally had thought. I began writing a simplified overview of the spec that I wished had existed when I was first learning this. I published it on my website as a blog post called “OAuth 2.0 Simplified” (https://aaronparecki.com/oauth-2-simplified/). This post is now viewed hundreds of thousands of times each year. It is clear that people know OAuth 2.0 is the right choice for securing their APIs, and are looking for resources to help understand it.

I had been wanting to expand this blog post into a more comprehensive guide to OAuth servers, and in 2016, I was put in touch with Okta, and we published the first version of this new guide to OAuth on oauth.com. In 2017, we collaborated on publishing the print edition of the book, and have published revised editions in 2018 and 2020.

My hope is that this book makes OAuth 2.0 more approachable, and gives you a solid foundation of knowledge that you’ll need as you continue to work with technologies on the Web.

## Background

Before OAuth, a common pattern for granting access to your account to a third-party application was to simply give it your password and allow it to act as you. We commonly saw this with Twitter apps which would ask for your Twitter password in order to give you some stats on your account, or would ask to be able to tweet something from your account in exchange for something of value.

This pattern of applications obtaining user passwords obviously has a number of problems. Since the application would need to log in to the service as the user, these applications would often store users’ passwords in plain text, making them a target for harvesting passwords. Once the application has the user’s password, it has complete access to the user’s account, including having access to capabilities such as changing the user’s password! Another problem was that after giving an app your password, the only way you’d be able to revoke that access was by changing your password, something that users are typically reluctant to do.

Naturally, many services quickly realized the problems and limitations of this model, and sought to solve this quickly. Many services implemented things similar to OAuth 1.0. Flickr’s API used what was called “FlickrAuth” which used “frobs” and “tokens”. Google created “AuthSub”. Facebook opted to issue each application a secret, and require the application sign each request with an md5 hash with that secret. Yahoo created “BBAuth” (Browser-Based Auth). The result was a wide variety of solutions to the problem, completely incompatible with each other, and often failing to address certain security considerations.

Around November 2006, Blaine Cook, chief architect at Twitter, was looking for a better authentication method for the Twitter API, something that didn’t require users giving out their Twitter passwords to third-party apps.

> We want something like Flickr Auth / Google AuthSub / Yahoo! BBAuth, but published as an open standard, with common server and client libraries, etc.
> Blaine Cook, April 5, 2007

In 2007, a group of people working on the development of OpenID got together and created a mailing list to produce a proposal for a standard for API access control that could be used by any system, regardless of whether it used OpenID. This original group included Blaine Cook, Kellen Elliott-McCrea, Larry Halff, Tara Hunt, Ian McKeller, Chris Messina, and a few others.

In the following months, several people from Google and AOL got involved, wanting to support the effort as well. By August 2007, the first draft of the OAuth 1 spec was published, along with several implementations of API clients working against Twitter’s privately-launched prototype of their OAuth API. Eran Hammer joined the project, eventually taking over as community chair and editor of the spec. By the end of the year, the community published 7 updated drafts and the OAuth Core 1.0 spec was declared final at the Internet Identity Workshop.

Over the next couple years, work on the OAuth spec moved to an IETF working group, where an effort to publish OAuth 1.1 was started. In November 2009, the editor proposed to drop work on the 1.1 revision and instead focus on a more significantly different 2.0 version.

The OAuth 2.0 spec started out as an effort to simplify and clear up many of the aspects of OAuth 1 that were difficult or confusing.

While several companies had implemented OAuth 1 APIs (namely Twitter, and later Flickr), there are some use cases, such as mobile applications, that cannot be safely implemented in OAuth 1. The goal of OAuth 2.0 was to take the knowledge learned from the first implementations of OAuth 1 and update it for the emerging mobile application use case, as well as to simplify aspects that were confusing to consumers of the APIs.

Work on the OAuth 2.0 spec began in the IETF working group, with Eran Hammer and several others named as editors of the spec. While the effort began on a strong note, it quickly became apparent that people in the group had very different goals with the spec.

The source of the contentions around the development of the OAuth 2.0 framework stemmed from the unbridgeable conflicts between the web and enterprise worlds. As work on the spec continued, most of the contributors in the web community left to go implement their products, leaving only the enterprise crowd to finish the spec.

In July 2010, the draft 10 was published, and no new drafts were published until December that year. Draft 10 still had a few people in the web community contributing, and so the spec was coming along nicely. The result was that most of the services that decided to implement an OAuth 2.0 API were reading draft 10. Most of the implementations at the time (Facebook, Salesforce, Windows Live, Google, Foursquare, etc) were all doing roughly the same thing. After launching their APIs they rarely went back and updated to newer drafts of OAuth 2.

Over the next 22 revisions of the standard, the web and enterprise contributors continued to disagree on fundamental issues. The only way to resolve the disagreements and continue making progress was to pull out the conflicting issues and put them into their own drafts, leaving holes in the spec that were called “extensible”. By the final draft, so much of the core was pulled into separate documents, that the core document was renamed from being a “protocol” to being a “framework,” and a disclaimer was added that “this specification is likely to produce a wide range of non-interoperable implementations.”

In 2012, Eran Hammer, the primary editor of the OAuth 2.0 standard, decided he could no longer contribute to the standard and officially withdrew his name and left the working group. Naturally this stirred up a lot of attention in what was going on with the standard, which he did a good job of addressing in blog posts and at one final conference in Portland, Oregon. He ended his blog post with “I’m hoping someone will take 2.0 and produce a 10 page profile that’s useful for the vast majority of web providers.”

Today, if someone wants to implement OAuth 2.0 for their web service, they need to synthesize information from a number of different RFCs and drafts. The standard itself does not require a token type, and does not require any specific grant types. This means implementers must decide which they will be supporting. The standard does not even give any guidance on token string size, which ends up being one of the first questions every implementer asks when they get started. Implementers must also read the security guidance and cautions in the document and understand the implications of the decisions they are forced to make.

Interestingly, most of the web services that do implement OAuth 2.0 for their APIs come to many of the same decisions, and so most of the OAuth 2.0 APIs in existence look very similar. This book is a guide to building OAuth 2.0 APIs, with concrete recommendations based on the majority of the live implementations.
