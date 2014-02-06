# git-trello-hook
A github/gitlab webhook script written by python inspired by ruby gem [git-trello](https://github.com/zmilojko/git-trello).

#Instructions

    $pip -r requirements.txt

    # Update these placeholders in git-trello-hook.py
    TRELLO_CONFIG = {
        'api_key': 'TRELLO_API_KEY',
        'oauth_token': 'TRELLO_OAUTH_TOKEN_FOR_BOARD',
        'board_id': 'BOARD_ID',
        'list_id_in_progress': 'LIST_ID',
        'list_id_done': 'LIST_ID',
    }

    WEBHOOK_CONFIG = {
        'host': '0.0.0.0',
        'port': 7343
    }

    # Open your github/gitlab repo settings, add an webhook URL according to your configs.
    e.g. For heroku deployment, add url https://git-trello-test.herokuapp.com/webhook
    e.g. For self hosting, add url https://your-ip-address:port/webhook

    $git commit -a -m "Fix [card #1]"
    $git push

    # git-trello will move Card #1(View Card, Card index is on the right bottom corner) from `list_id_in_progress` to `list_id_done` and append with your git commit url.

###`API_KEY`
https://trello.com/1/appKey/generate

###`OAUTH_TOKEN`
This is not so well explained in Trello, but I understood that you need to authorize the app with API_KEY to access each board separatelly. To do that:

https://trello.com/1/authorize?response_type=token&name=[BOARD+NAME+AS+SHOWN+IN+URL]&scope=read,write&expiration=never&key=[YOUR+API_KEY+HERE]

where [YOUR+API_KEY+HERE] is the one you entered in the previous step, while [BOARD+NAME+AS...] is, well, what it says. If your board url is 

https://trello.com/b/XLvlTFVA/git-trello

then you should type in "git-trello".


###`TRELLO_BOARD_ID`
It is the end of the URL when viewing the board. For example, for https://trello.com/b/XLvlTFVA/git-trello, board_id is XLvlTFVA.

###`LIST_ID_IN_PROGRESS and LIST_ID_IN_DONE`
List IDs seem to be a (board id + list index), where all are treated as hex numbers. However, this is undocumented.

Safe way to find a list ID is to open a card from the list, click the More link in the bottom-right corner, select Export JSON and find the idList.

Post receive will move all referenced cards to the LIST_ID_IN_PROGRESS, unless they are referenced by Close or Fix word, in which case it will move them to the LIST_ID_IN_DONE.

#Examples

Example [Trello board](https://trello.com/b/Yl6AN4Pj/git-trello-test)
Example [git repo](https://github.com/hewigovens/git-trello-test)

#TODOs
* easy configuration
* easy deployment

#Credits

[git-trello](https://github.com/zmilojko/git-trello)