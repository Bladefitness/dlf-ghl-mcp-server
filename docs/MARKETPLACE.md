# Publishing to GHL Marketplace

## What Changes When You Publish

Right now the GHL app is a Private Integration — it only works for sub-accounts within your own agency. Publishing to the Marketplace means any GHL user can install it.

**What stays the same:**
- The entire server codebase
- The `/callback` endpoint already handles external installs
- D1 stores their token the same way as internal accounts
- `resolveClient` routes their calls identically

**What changes:**
- GHL reviews and approves the app
- Anyone who installs it appears in your D1 automatically
- You have API access to their account from that point on
- No PIV tokens, no manual setup — fully automated

## Install URL

```
https://marketplace.gohighlevel.com/oauth/chooselocation?response_type=code&redirect_uri=https%3A%2F%2Fdlf-agency.skool-203.workers.dev%2Fcallback&client_id=69adfc954f90be0751e99787-mmictftt&scope=oauth.write+oauth.readonly+locations.write+locations.readonly+users.readonly+users.write+companies.readonly+documents_contracts%2Flist.readonly+documents_contracts%2FsendLink.write+documents_contracts_template%2FsendLink.write+documents_contracts_template%2Flist.readonly+marketplace-installer-details.readonly+marketplace-external-auth-migration.write+calendars.write+calendars.readonly+calendars%2Fevents.readonly+calendars%2Fgroups.readonly+calendars%2Fevents.write+calendars%2Fresources.readonly+calendars%2Fgroups.write+calendars%2Fresources.write+campaigns.readonly+associations.write+associations.readonly+associations%2Frelation.readonly+associations%2Frelation.write+conversations%2Fmessage.readonly+conversations%2Freports.readonly+conversations%2Fmessage.write+conversations%2Flivechat.write+conversation-ai.readonly+conversation-ai.write+conversations.write+conversations.readonly+invoices.readonly+invoices.write+invoices%2Fschedule.readonly+invoices%2Fschedule.write+invoices%2Ftemplate.write+invoices%2Ftemplate.readonly+invoices%2Festimate.readonly+invoices%2Festimate.write+links.readonly+links.write+custom-menu-link.readonly+custom-menu-link.write+contacts.readonly+contacts.write+locations%2FcustomValues.readonly+locations%2FcustomValues.write+locations%2FcustomFields.readonly+locations%2FcustomFields.write+locations%2Ftasks.readonly+locations%2Ftasks.write+locations%2Ftags.readonly+locations%2Ftags.write+locations%2Ftemplates.readonly+saas%2Fcompany.read+saas%2Fcompany.write+saas%2Flocation.read+saas%2Flocation.write+opportunities.readonly+opportunities.write+voice-ai-agents.readonly+voice-ai-agents.write+voice-ai-agent-goals.readonly+voice-ai-agent-goals.write+agent-studio.readonly+knowledge-bases.readonly+knowledge-bases.write+agent-studio.write+socialplanner%2Foauth.readonly+socialplanner%2Foauth.write+socialplanner%2Fpost.write+socialplanner%2Faccount.write+socialplanner%2Fcsv.readonly+socialplanner%2Fcsv.write+socialplanner%2Fcategory.readonly+socialplanner%2Ftag.readonly+socialplanner%2Fstatistics.readonly+socialplanner%2Fcategory.write+socialplanner%2Fpost.readonly+socialplanner%2Faccount.readonly+socialplanner%2Ftag.write+workflows.readonly+surveys.readonly+forms.readonly+forms.write+courses.write+courses.readonly+blogs%2Fcheck-slug.readonly+blogs%2Fpost.write+blogs%2Fpost-update.write+blogs%2Fcategory.readonly+blogs%2Fauthor.readonly+blogs%2Fposts.readonly+blogs%2Flist.readonly+phonenumbers.read+charges.readonly+charges.write+twilioaccount.read+numberpools.read+businesses.readonly+businesses.write+objects%2Fschema.readonly+objects%2Fschema.write+objects%2Frecord.readonly+objects%2Frecord.write+lc-email.readonly+recurring-tasks.write+recurring-tasks.readonly+medias.readonly+medias.write+funnels%2Fredirect.readonly+funnels%2Fpage.readonly+funnels%2Ffunnel.readonly+funnels%2Fpagecount.readonly+funnels%2Fredirect.write+payments%2Forders.readonly+payments%2Forders.write+payments%2Forders.collectPayment+payments%2Fintegration.readonly+payments%2Fintegration.write+payments%2Ftransactions.readonly+payments%2Fcustom-provider.write+payments%2Fcustom-provider.readonly+payments%2Fcoupons.write+payments%2Fsubscriptions.readonly+payments%2Fcoupons.readonly+products.write+products.readonly+products%2Fprices.readonly+products%2Fprices.write+products%2Fcollection.readonly+products%2Fcollection.write+snapshots.readonly+snapshots.write+store%2Fshipping.readonly+store%2Fshipping.write+store%2Fsetting.readonly+store%2Fsetting.write+emails%2Fbuilder.write+emails%2Fbuilder.readonly+emails%2Fschedule.readonly+wordpress.site.readonly+brand-boards%2Fdesign-kit.write+brand-boards%2Fdesign-kit.readonly+voice-ai-dashboard.readonly&version_id=69adfc954f90be0751e99787
```

## Steps to Publish

1. Go to `https://app.gohighlevel.com/integration/69adfc954f90be0751e99787/versions/69adfc954f90be0751e99787`
2. Move the app version from **Draft** to **Published**
3. Once published, GHL assigns a real version UUID — update `version_id` in the install URL
4. Submit to GHL Marketplace for review if you want public distribution

## What Happens When a Client Installs

1. Client opens your install URL and authorizes
2. GHL redirects to `/callback`
3. Their location token is stored in your D1 automatically
4. They appear in `ghl_list_sub_accounts` immediately
5. You can make API calls to their location from that point on
