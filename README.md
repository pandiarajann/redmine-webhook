# Redmine WebHook Plugin

A [Redmine](http://www.redmine.org) plugin that posts data to a configured webhook URL upon creating and updating issues.

## Author
* @suer (original author)
* @phanan (rewriting and adding some features)
* @pandiarajann (Added trigger features when an issue is closed)

## Install
Type below commands:

    $ cd $REDMINE_ROOT/plugins
    $ git clone git@github.com:phanan/redmine_webhook.git
    $ rake redmine:plugins:migrate RAILS_ENV=production

Then, restart your Redmine.

## Post Data Examples
### Issue opened
```
{
  "payload": {
    "action": "opened",
    "issue": {
      "id": 1,
      "subject": "A sample bug",
      "description": "Lorem ipsum dolor sic amet.",
      "created_on": "2015-03-06T04:23:42Z",
      "updated_on": "2015-03-07T10:00:59Z",
      "closed_on": null,
      "root_id": 1,
      "parent_id": null,
      "done_ratio": 0,
      "start_date": "2015-03-02",
      "due_date": "2015-03-20",
      "estimated_hours": 15,
      "is_private": false,
      "lock_version": 14,
      "project": {
        "id": 1,
        "identifier": "playground",
        "name": "Playground",
        "description": "A sample playground project",
        "created_on": "2015-03-06T02:51:48Z",
        "homepage": ""
      },
      "status": {
        "id": 1,
        "name": "New"
      },
      "tracker": {
        "id": 2,
        "name": "Feature"
      },
      "priority": {
        "id": 3,
        "name": "High"
      },
      "author": {
        "id": 1,
        "login": "admin",
        "mail": "admin@example.net",
        "firstname": "Redmine",
        "lastname": "Admin",
        "identity_url": null,
        "icon_url": "http:\/\/www.gravatar.com\/avatar\/cb4f282fed12016bd18a879c1f27ff97?rating=PG&size=50"
      },
      "assignee": {
        "id": 5,
        "login": "demo",
        "mail": "demo@example.net",
        "firstname": "Demo",
        "lastname": "User",
        "identity_url": null,
        "icon_url": "http:\/\/www.gravatar.com\/avatar\/0e5601057dfe4b0fa94611f1fad4fb95?rating=PG&size=50"
      },
      "watchers": [
        {
          "id": 1,
          "login": "admin",
          "mail": "admin@example.net",
          "firstname": "Redmine",
          "lastname": "Admin",
          "identity_url": null,
          "icon_url": "http:\/\/www.gravatar.com\/avatar\/cb4f282fed12016bd18a879c1f27ff97?rating=PG&size=50"
        }
      ]
    },
    "url": "http:\/\/localhost:3000\/issues\/1"
  }
}
```

### Issue updated
```
{
  "payload": {
    "action": "updated",
    "issue": {
      "id": 1,
      "subject": "A sample bug",
      "description": "Lorem ipsum dolor sic amet.",
      "created_on": "2015-03-06T04:23:42Z",
      "updated_on": "2015-03-07T10:00:59Z",
      "closed_on": null,
      "root_id": 1,
      "parent_id": null,
      "done_ratio": 100,
      "start_date": "2015-03-02",
      "due_date": "2015-03-20",
      "estimated_hours": 15,
      "is_private": false,
      "lock_version": 14,
      "project": {
        "id": 1,
        "identifier": "playground",
        "name": "Playground",
        "description": "A sample playground project",
        "created_on": "2015-03-06T02:51:48Z",
        "homepage": ""
      },
      "status": {
        "id": 2,
        "name": "In Progress"
      },
      "tracker": {
        "id": 2,
        "name": "Feature"
      },
      "priority": {
        "id": 2,
        "name": "Normal"
      },
      "author": {
        "id": 1,
        "login": "admin",
        "mail": "admin@example.net",
        "firstname": "Redmine",
        "lastname": "Admin",
        "identity_url": null,
        "icon_url": "http:\/\/www.gravatar.com\/avatar\/cb4f282fed12016bd18a879c1f27ff97?rating=PG&size=50"
      },
      "assignee": {
        "id": 5,
        "login": "demo",
        "mail": "demo@example.net",
        "firstname": "Demo",
        "lastname": "User",
        "identity_url": null,
        "icon_url": "http:\/\/www.gravatar.com\/avatar\/0e5601057dfe4b0fa94611f1fad4fb95?rating=PG&size=50"
      },
      "watchers": [
        {
          "id": 1,
          "login": "admin",
          "mail": "admin@example.net",
          "firstname": "Redmine",
          "lastname": "Admin",
          "identity_url": null,
          "icon_url": "http:\/\/www.gravatar.com\/avatar\/cb4f282fed12016bd18a879c1f27ff97?rating=PG&size=50"
        }
      ]
    },
    "journal": {
      "id": 28,
      "notes": "",
      "created_on": "2015-03-07T10:00:59Z",
      "private_notes": false,
      "author": {
        "id": 5,
        "login": "demo",
        "mail": "demo@example.net",
        "firstname": "Demo",
        "lastname": "User",
        "identity_url": null,
        "icon_url": "http:\/\/www.gravatar.com\/avatar\/0e5601057dfe4b0fa94611f1fad4fb95?rating=PG&size=50"
      },
      "details": [
        {
          "id": 56,
          "value": 3,
          "old_value": 1,
          "prop_key": "status_id",
          "property": "attr"
        },
        {
          "id": 57,
          "value": 3,
          "old_value": 2,
          "prop_key": "priority_id",
          "property": "attr"
        }
      ]
    },
    "url": "http:\/\/localhost:3000\/issues\/1#change-28"
  }
}
```
## Trigger Examples

Triggers a jenkins build job when an issue is closed using the specified URL with the token and parameters. The keyword is "closed" while closing the issue.

### Build with parameters
http://<(hostname)>:<(port)>/job/<(jobname)>/buildWithParameters?token=<(token)>

Note:
The parameters are programatically appended to the URL.

List of parameters appended:
* project_name
* subject
* status
* tracker
* priority
* created_on
* closed_on
* (All the custom field values in sequence). Use the exact keywork of the customfield name as specified in the redmine.

## Requirements
* Redmine >= 2.4 (not tested with 3.x)
