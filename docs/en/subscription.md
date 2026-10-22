# iOS/iPadOS subscriptions

## Android preparation (not released)

Android 1.0.0 (34) is being prepared with the same free-reading and monthly-editing model, using Google Play rather than Apple. The intended US price is $0.99/month with a two-week trial for eligible users. Actual local prices and eligibility will come from Google Play. The published Android 0.1 (30) build does not include this change; product configuration and real purchase testing are still pending.

The upcoming Android Settings → Subscription page supports purchase, Restore Subscription and Manage Subscription directly through Google Play Billing. It requires no Aster account or developer-operated verification service. Restore uses the Google Play account used to purchase. Apple and Google Play purchases are not shared across platforms.

Offline or failed checks retain the last successfully confirmed editing access. Access may continue past actual expiry or refund until the next successful check, and the app does not display an inferred expiry date. Losing editing access preserves files and rejected drafts, while reading and ordinary cloud synchronization remain available. See the [privacy policy](../../PRIVACY.md) for the data boundary.

[简体中文](../subscription.md) · [Home](../../README.en.md)

## Release status

Version 1.0.0 (34) and its first monthly subscription were submitted to App Review on October 17, 2026. They are waiting for review and have not launched on the App Store. This page describes that version's subscription plan, which is not included in 0.1 (30). The current Android version is unaffected.

The initial release excludes China mainland and the European Union. Plans for the other selected regions are unchanged.

The planned auto-renewable monthly subscription costs $0.99 in the United States. Other available storefronts use local-currency App Store prices. Apple's purchase screen is authoritative for the actual price, taxes, and trial eligibility; prices do not track exchange rates every month.

## Trial, purchase, and restoration

Eligible users receive a two-week free trial after confirming the subscription with Apple. It then renews monthly unless cancelled beforehand. Apple's confirmation screen describes billing and cancellation deadlines. Apple determines eligibility per subscription group; reinstalling does not restart the trial.

Open Settings → Subscription to see local pricing, purchase, or restore purchases. Restore using the Apple Account used for the original purchase. Active subscribers see an information card with the current monthly plan and access deadline, rather than another purchase-looking button. Manage Subscription opens Apple's subscription management, and restoration remains available. Cancelling renewal preserves editing access until the current paid or trial period ends; the access deadline is not a promise of renewal.

## Files after expiration

Without an active subscription or trial, existing files, source, attachments, and indexed views remain readable. Source text can still be selected and copied. Creating, editing, deleting, moving, archiving, and restoring file versions require access. Expiration never automatically deletes or moves files or converts them into a proprietary format.

Cloud sync and existing system-integration synchronization continue under the original connection, permission, and conflict rules, including externally made changes. Settings, navigation, and connection management remain available. Removing local copies still requires your explicit confirmation.

If access expires during source editing or Journal Capture, saving is blocked while the source text or capture draft remains available. Note and diary-description forms do not close themselves when their save is rejected. Copy the text or save the draft before closing the editor. Unsaved text is not a committed Org edit.

[Privacy policy](../../PRIVACY.md) · [Apple standard terms of use](https://www.apple.com/legal/internet-services/itunes/dev/stdeula/)

Support: chenyh572@gmail.com
