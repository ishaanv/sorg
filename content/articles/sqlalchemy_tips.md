---
hook: Some neat yet underused features of SQLAlchemy
location: Sydney
published_at: 2017-08-25T15:25:07Z
title: Three SQLAlchemy Tips
---

I've been using SQLAlchemy (SQLA) for about a year so I'd thought I'd share
three neat ways I've discovered it being used.

## Created at, updated at columns

``` python
class TimestampMixin(object):
    @declared_attr
    def created_at(cls):
        return Column(types.DateTime,
                      default=datetime.datetime.utcnow,
                      nullable=False,
                      server_default=expression.text(
                        'CURRENT_TIMESTAMP'))
    @declared_attr
    def updated_at(cls):
        return Column(types.DateTime, default=datetime.datetime.utcnow,
                      onupdate=datetime.datetime.utcnow,
                      nullable=False, server_default=expression.text('CURRENT_TIMESTAMP')


class StampedMixin(object):
    created_on = Column(UTCDateTime, default=datetime.utcnow, nullable=False)
    modified_on = Column(UTCDateTime, default=datetime.utcnow,
                         onupdate=datetime.now, nullable=False)
```

Breakdown: Mixins reduce code redundancy. For example, whenever we require a `created_on` and a `modified_on` column in a table, all we have to do is inherit from the mixin.



## Triggers

This feature is often overlooked, however, in my opinion, is very useful.

``` python
@event.listens_for(User, 'after_insert')
def on_insert_transaction(mapper, connection, target):
    account = Account(user_id=target.id)
    connection.add(account) # something like that
```

 

### Triggers + celery



## Some other thing



 
 
