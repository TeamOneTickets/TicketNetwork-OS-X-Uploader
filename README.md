Hackery to Upload to TicketNetwork via OS X
===

It appears that TicketNetwork refuses to allow the awesome [Ticket Evolution Auto-Uploader](http://ticketevolution.com/products/auto-upload-inventory-tool/) to upload to TicketNetwork so we threw together some hackery and kludgery to get the job done. We have been using it for over a year.

This requires the awesome [Fake.app](http://fakeapp.com/) from [Todd Ditchendorf](https://twitter.com/#!/itod).

TicketNetwork Uploader.fakeworkflow
---
This is the workflow file used by Fake.app.

Open it in Fake.app and look for the first occurrence of `Set Values of HTML Form` and change `tbPassword` and `tbUsername` to match your TicketNetwork user credentials.

Find the second occurrence of `Set Values of HTML Form` and change `fileInventory` to the full path to your inventory file. *This inventory file needs to be in a folder with NO other files in there.* e.g.:`/Users/teamone/Dropbox/InventoryFiles/TeamOneTickets/TicketNetwork/TicketNetwork.csv`

Run Ticket Network Uploader
---
This is an AppleScript.

Unfortunately, you cannot (AFAIK) tell Fake.app to run a workflow periodically, but AppleScript can so this is essentially a wrapper to do that.

You will need to open this file and edit the first line to match your `/Full/Path/To/TicketNetwork Uploader.fakeworkflow`.

net.teamonetickets.pos.upload.ticketnetwork
---
This file tells [launchd](http://en.wikipedia.org/wiki/Launchd) to watch the folder that contains your inventory file for changes. (This is why your .csv file needs to be the only thing in that folder.) Any time anything in that folder changes — presumably when you output a new .csv file — the `Run Ticket Network Uploader` AppleScript will open and tell Fake.app to run the TicketNetwork Uploader.fakeworkflow.

This file needs to be placed into `/Library/LaunchDaemons/` and must be owned by root. If you’re not comfortable messing around with `launchd` then you may want to download [Lingon](http://www.peterborgapps.com/lingon/).
