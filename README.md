# RT-Extension-Slack
Integration with Slack webhooks

# DESCRIPTION
This module is a modified version of Anhdrew's original RT4 intergration with Slack. Since we are already at RT5 version, we need to remove several contraints so that we can install this plugin and use it as well.

The module works with the *Mattermost* server as well.

# RT VERSION
Works with RT 4.2.0 and newer

# INSTALLATION
    perl Makefile.PL
    make
    make install

May need root permissions

Edit your /opt/rt4/etc/RT_SiteConfig.pm
If you are using RT 4.2 or greater, add this line:

	Plugin('RT::Extension::Slack');

For RT 4.0, add this line:

	Set(@Plugins, qw(RT::Extension::Slack));

Clear your mason cache
		rm -rf /opt/rt4/var/mason_data/obj

Restart your webserver

# CONFIGURATIONS
Edit your /opt/rt4/etc/RT_SiteConfig.pm to include:
    Set($SlackWebhookURL, "slack-hook-url");

# USAGE

Basic scrip use. 

Action: User Defined

Add this code to your scrip's "Custom action preparation code":


```
my $text; 
my $requestor; 
my $ticket = $self->TicketObj; 
my $queue = $ticket->QueueObj; 
my $url = join '', 
	RT->Config->Get('WebPort') == 443 ? 'https' : 'http', 
	'://', 
	RT->Config->Get('WebDomain'), 
	RT->Config->Get('WebPath'), 
	'/Ticket/Display.html?id=', 
	$ticket->Id; 
 
$requestor = $ticket->RequestorAddresses || 'unknown'; 
$text = sprintf('New ticket <%s|#%d> by %s: %s', $url, $ticket->Id, $requestor, $ticket->Subject); 

RT::Extension::Slack::Notify(text => $text); 
```

The call to ``RT::Extension::Slack::Notify`` takes further args to fill the payload.

```
RT::Extension::Slack::Notify(text => $text, channel => "support-team", username => "Helpdesk"); 
```

# AUTHORS
[Maciek] (http://www.gossamer-threads.com/lists/rt/users/128413#128413)  
Andrew Wippler 
Michal Cech
    

# LICENSE AND COPYRIGHT
    The MIT License (MIT)

    Copyright (c) 2015 Andrew Wippler

    Permission is hereby granted, free of charge, to any person obtaining a
    copy of this software and associated documentation files (the
    "Software"), to deal in the Software without restriction, including
    without limitation the rights to use, copy, modify, merge, publish,
    distribute, sublicense, and/or sell copies of the Software, and to
    permit persons to whom the Software is furnished to do so, subject to
    the following conditions:

    The above copyright notice and this permission notice shall be included
    in all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
    OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
    MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
    IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
    CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
    TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
    SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

