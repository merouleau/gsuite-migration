# gsuite-migration

This repo is a place to manage our family's migration from GSuite Legacy Free to external email hosting and personal Gmail and Google accounts. It's **PUBLIC** to share our work with others and to avoid paying for GitHub subscriptions, and that implies

**RULE #1**: Nothing sensitive can be shared here. We'll use LastPass for passwords and private keys that we may or may not decide we need to share.

## Background

In 2007 we accepted Google's offer of a perpetually free Google Apps for Domains service with 50 user accounts. At some point, our account limit became 100, and subsequently, new subscribers to the free offer were limited to ten accounts. In 2012, Google stopped accepting new subscribers to the free service. Over the years, Google Apps for Domains became GSuite, and the free service was named GSuite Legacy Free Edition (GLFE).

Now GSuite is becoming Workspace. With that transition, Google will charge full price for the service that it promised would be perpetually free. While offering no specific terms, Google has hinted about a discount to non-business GLFE customers with at most ten accounts.

Our family uses GLFE to support a shared identity on the Internet, has nine active accounts, and expects to need more as marriages occur and children are born and mature. The 50 accounts promised in 2007 are enough; ten are not.

While trusting Google to keep its word was a mistake, we did hedge our bets slightly in a way that enables a superior solution to our problem. Our GLFE service ties to a subdomain (ga.rouleaufam.org), and rouleaufam.org is an alias domain. Our GLFE accounts are user@ga.rouleaufam.org, but our public identities for email and sharing are user@rouleaufam.org. The distinction allows us to make new user@rouleaufam.org personal Google accounts and leave the ga.rouleaufam.org service in perpetual suspension to support legacy authentication needs.

If our accounts and public identities were identical, the solution we're implementing would require us to cancel the GLFE service entirely and lose the ability to authenticate to those accounts. The result would be stranded assets:

1. _Sign in with Google_ accounts.
2. Still-supported services like Photos and YouTube.
3. Paid content like movies from the Play Store.
4. Android devices - phones, tablets, streamers, and TVs - that we forget to reset before the cancellation.

## Concerns

1. __Email addresses in the rouleaufam.org domain__. We don't want to suffer the inconvenience of changing our addresses and updating the thousands of accounts to which they are tied. Also, we like that our addresses advertise our family rather than a corporation.

2. __Email vs. Google identity__. With rouleaufam.org as a user alias domain for ga.rouleaufam.org, we enjoy unity of identity across email and all Google services. Other Google users can share with our email addresses (e.g., marc@rouleaufam.org). If I must tell people that my share address is marc.rouleau@gmail.com when they know me as marc@rouleaufam.org, overall usability is impaired. More under Options.

3. __GLFE cancellation vs. suspension__. If we cancel GSuite, we strand assets as described above under ___Background___. Suspension preserves access and is the superior choice.

4. __User account suspension__. Suspending users within a Workspace account has different effects and does not relieve us of the cost - it's useless. If we upgrade to buy time or for the long haul, we must weigh savings from each potential user account cancellation against the risk of stranding assets.

5. __Devices (cancellation)__. As [this guy's experience](https://support.google.com/a/thread/8686383/leaving-gsuite-experience) shows, deleting an account can brick your devices. It's a pitfall of Factory Reset Protection. If a device - phone, tablet, TV - is reset while not connected to the account which initially set it up, it must authenticate to that account during any subsequent setup. So those old backup devices that you might use someday? You have to boot them up, connect them to the account, and reset them BEFORE losing access to the account. And that Android TV? It's recycling fodder if you delete the account before resetting it.

6. __Devices (suspension)__. When you sign into a dormant device, Google often denies the sign-in and emails a Security alert to the primary and recovery addresses. Only after you click the Check activity button will it allow a sign-in. If Gmail for the primary is suspended, that message won't arrive. Make sure you have a recovery email configured for your @ga.rouleaufam.org account!

7. __Photos (suspension)__. rclone can copy these, but I see no way to copy or move share grants. Fortunately, Google says Photos will continue to be accessible during suspension - presumably, this accessibility will include pre-existing shares?

8. __Drive (suspension)__. Drive is only available for Takeout after suspension (7/1/22 earliest) and not at all 60 days later, so we'll need to copy with rclone and identify and reestablish existing shares both with and by the account. Note that Docs and Sheets are part of Drive! And while rclone can copy the Shared with me folder, it's a poor substitute for acquiring re-shares as all those files will consume quota and lack connection to the owners' copies.

9. __Gmail (suspension)__. Before suspension (or individual account cancellation), we must migrate Gmail. We need to learn which of POP3, IMAP, and Takeout is the best tool. When I canceled Gene's account, I used POP3 to get his messages to Judy, but there were only around 250 messages of note.

## Options

I believe there are two ways to maintain unity of identity (item 2 above) between our rouleaufam.org email addresses and our Google services. The first is to upgrade to Workspace and continue as we have been. This may be prohibitively expensive if/as our number of accounts grows.

The second way is to host rouleaufam.org elsewhere and create Google accounts via the Use my current email address instead method. And assuming that we all want to continue using Gmail, we would each have a second Google account purely for Gmail. So in my case, I would process email in my marc.rouleau@gmail.com account, but I wouldn't use that address for anything other than the recovery address for my primary marc@rouleaufam.org Google account. My Android devices would be set up with the marc@rouleaufam.org account, and that's where my Photos and Drive assets would reside.

As a side note, Google accounts created using the Use my current email address instead method have the same capabilities as gmail.com ones - to verify, I made one for marc@apoe4.info and added it to my family group for YouTube Premium.

In this second scenario, we would allow ga.rouleaufam.org to be suspended. Those accounts would remain available for device authentication and accessing pre-transition Photos shares. We would point the mail exchanger records in DNS to the new host to deliver any stray messages addressed to us @ga.rouleaufam.org.

For hosting rouleaufam.org email, I suggest MDDHosting, a cPanel-based web hosting service with which I have a good relationship via my work with ApoE4.Info. Generally, cPanel services are oriented toward websites, but they throw in the email features we need as freebies. We get unlimited accounts, aliases, and mailing lists for the monthly website fee. cPanel even supports Gmail's "+" feature (e.g., marc+anything@rouleaufam.org gets delivered to marc@rouleaufam.org). The cost is $15/month, which I would cover.

There are a few ways to integrate with Gmail (but only one right way!):

1. __No email account at the host__. Just forward yourname@rouleaufam.org to your Gmail account. This is simple and fast, but it doesn't allow you to check for misclassified spam at the host. Not recommended.

2. __An email account at the host AND a forwarding entry__. This is just as fast as option 1, but you can check the spam folder. On the downside, you have to manage disk usage as we would all be sharing just 25 GB. Not recommended.

3. __An email account at the host AND a forwarding entry AND a POP3 connection in Gmail__. This non-obvious solution relies upon Gmail's outstanding deduplication capability. It's receiving each message twice, but it only presents it to us once! This is the best of all worlds because it's fast, and the POP3 connection empties the mailbox automatically. It does not bring the spam across, so you might need to log into the hosting account occasionally to check the spam folder.
