.. _settings:

Settings
========


REMINDERS
---------

This app is driven by a list of callables and associated messages
configured by this setting. Here is an example::

    from emailconfirmation.reminders import confirmed
    
    REMINDERS = {
        "profile_completed": {
            "test": "profiles.reminders.completed",
            "message": "You have only completed %(percentage)s%% of your <a href="%(url)s">profile</a>.",
            "dismissable": "permanent"
        },
        "email_confirmed": {
            "test": lambda user: confirmed(user),
            "message": "Please <a href="%(url)">confirm</a> your email address.",
            "dismissable": "no"
        }
    }

Valid values for the `dismissable` key are `permanent`, `session`, and `no`. If
left out of the settings it will default to `session`. As you might have guessed,
this controls whether or not a user can dismiss a reminder and if they do, whether
it is dismissed for the duration of their session or for good.


Callable API
^^^^^^^^^^^^

This callables may be provided by third-party apps or may be defined by you,
the site developer. In either case, they should follow the following
conventions::

    def name(user):
        if there_is_stuff_for(user):
            return build_up_dict_for(user)

The message in the tuple with this callable will need to know what data is
being supplied by the callable. If there isn't a reminder, then the callable
should return None.
