# Event Bus Service (Beta)

Event Bus service is a powerful communication system that helps your blocklet's components work together seamlessly. Imagine it as a smart messaging center that:

- Lets different blocklets (which are like mini-apps within your system) share information and notify each other about important changes
- Creates a smooth connection between your blocklets and the main server that manages everything

For example, if you have a shopping blocklet and an inventory blocklet, the Event Bus would help them stay synchronized - when someone buys an item, the inventory blocklet would automatically know to update its stock count.

## Usage

### Define Events

To get started, you'll need to specify what kinds of messages (events) your blocklet can send. This is done by listing them in your `blocklet.yml` configuration file, which acts like a rulebook for your blocklet's behavior.

```yaml
events:
  - type: kitchen.full
    description: dummy event from kitchen sink blocklet
  - type: kitchen.empty
    description: dummy event from kitchen sink blocklet
  - type: blocklet.kitchen.event
    description: This event name is forbidden
```

Here are some important guidelines when naming your events:

- Think of event names like describing an action: start with what's happening (like "post" or "user") followed by what was done (like "published" or "updated"). For example: "post.published" or "user.loggedIn"
- Each event name needs to be unique in your blocklet - you can't have two events with the same name
- Don't use names that start with "blocklet." - these names are reserved for system use
- Make sure to write clear descriptions for your events - these will help other developers understand what your events do

### Publish Events

```js
const EventBus = require('@blocklet/sdk/service/eventbus');

EventBus.publish('kitchen.full', {
  id: '123', // optional, default is a random uuid
  time: new Date().toISOString(), // optional, default is current time
  data: { object: { message: 'Hello, world!' } },
})
  .then(() => {
    console.log('Event published');
  })
  .catch((err) => {
    console.error('Failed to publish event', err);
  });
```

Before your blocklet can send out any messages (events), you need to list them in your blocklet's configuration file (`blocklet.yml`) - think of this like registering what kinds of announcements your blocklet is allowed to make. This helps keep communication organized and prevents confusion about which blocklet is sending what messages.

### Receive Events

```js
const EventBus = require('@blocklet/sdk/service/eventbus');

EventBus.subscribe((event) => {
  console.log('received event from eventbus', event);
});
```

Your blocklet can listen for messages (events) from other blocklets and the server. However, it won't receive the specific events that it has declared in its own `blocklet.yml` file - this prevents a blocklet from receiving its own messages, which helps avoid confusion and feedback loops.

### Discover Events

Want to know what events are available in your blocklet? You can find a complete list at the `/.well-known/service/openevent.json` web address. This list shows all the events that your blocklet and other blocklets can send and receive - it's like a directory of all possible messages that can be shared between different parts of your application.

```shell
❯ curl -s https://bbqartjsky2iiebu2noxsg6gn7nf7ezacguwmx6q6ci.did.abtnet.io/.well-known/service/openevent.json | jq
{
  "openevent": "1.0.0",
  "info": {
    "description": "I can change this after elevated, great! haha"
  },
  "sources": [
    {
      "did": "zNKYyCsfQASyGp5Gcz3vmkRyLM73cRG13fVs",
      "description": "I can change this after elevated, great! haha",
      "version": "1.0.0",
      "logo": "https://bbqartjsky2iiebu2noxsg6gn7nf7ezacguwmx6q6ci.did.abtnet.io/.well-known/service/blocklet/logo"
    },
    {
      "did": "z8iZqG23gxzv6CbTmtWFAipHGLjPEha4BjAtE",
      "title": "Profile Demo",
      "description": "Blocklet that show profile of login user, can be combined by other blocklets.",
      "version": "1.14.17",
      "logo": "https://bbqartjsky2iiebu2noxsg6gn7nf7ezacguwmx6q6ci.did.abtnet.io/.well-known/service/blocklet/logo-bundle/z8iZqG23gxzv6CbTmtWFAipHGLjPEha4BjAtE?v=1.0.0"
    },
    {
      "did": "z8ia22AX1PovjTi1YQw8ChgsbeVExYsX4dPFt",
      "title": "Kitchen Sink",
      "description": "Demo blocklet that showing how blocklet works in ABT node",
      "version": "1.5.3",
      "logo": "https://bbqartjsky2iiebu2noxsg6gn7nf7ezacguwmx6q6ci.did.abtnet.io/.well-known/service/blocklet/logo-bundle/z8ia22AX1PovjTi1YQw8ChgsbeVExYsX4dPFt?v=1.0.0"
    }
  ],
  "events": [
    {
      "type": "blocklet.user.added",
      "description": "When a new user has onboarded, will receive the user info object",
      "source": "zNKYyCsfQASyGp5Gcz3vmkRyLM73cRG13fVs"
    },
    {
      "type": "blocklet.user.updated",
      "description": "When a user has updated/logged in, will receive the user info object",
      "source": "zNKYyCsfQASyGp5Gcz3vmkRyLM73cRG13fVs"
    },
    {
      "type": "blocklet.user.removed",
      "description": "When a user has removed, will receive the user did",
      "source": "zNKYyCsfQASyGp5Gcz3vmkRyLM73cRG13fVs"
    },
    {
      "type": "blocklet.user.profile.updated",
      "description": "When a user has updated his/her profile (full name, avatar, did-spaces, etc.)",
      "source": "zNKYyCsfQASyGp5Gcz3vmkRyLM73cRG13fVs"
    },
    {
      "type": "blocklet.user.permission.updated",
      "description": "When a user permission (passport, account, kyc) has changed",
      "source": "zNKYyCsfQASyGp5Gcz3vmkRyLM73cRG13fVs"
    },
    {
      "type": "dummy.event",
      "description": "This is a dummy event from profile demo",
      "source": "z8iZqG23gxzv6CbTmtWFAipHGLjPEha4BjAtE"
    },
    {
      "type": "kitchen.full",
      "description": "dummy event from kitchen sink blocklet",
      "source": "z8ia22AX1PovjTi1YQw8ChgsbeVExYsX4dPFt"
    },
    {
      "type": "kitchen.empty",
      "description": "dummy event from kitchen sink blocklet",
      "source": "z8ia22AX1PovjTi1YQw8ChgsbeVExYsX4dPFt"
    }
  ]
}
```
