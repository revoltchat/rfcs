# Slash Commands

<span style="color: grey">/dm</span> <span style="color: gold">user</span>: <code><span style="color: lightblue">@Zomatree</span></code> <span style="color: gold">message</span>: <code>asd</code>

https://www.figma.com/file/cDSUvArKH2pG0zv35na9El/Interactions?type=design&node-id=0%3A1&t=qSQJIFFjTD9hEFF6-1

## Parameter Types

- Text
    standard string text
   
- integer
    This will be bigint so sending and receiving them will be strings, converting to the correct type will be the bots responsability

- number
    Floating point number, like integer this will be a string

- boolean
    a true or false value

- Member
    member in the server

- Channel
    channel in the server, could be voice or text

- Role
    role in the server

- Date
    no time included, will be sent as a unix timestamp

- Time
    time of the day, will be sent in milliseconds from 00:00

- Datetime
    like Date but with a time included, will be sent as a unix timestamp

- Attachment
    Uploadable file, will be sent as a autumn attachment id

- VarArgs
    multiple args of varying length in one
 
- Group
    These should be able to be ran and not be required to ran only the subcommands

- Subcommand
    A subcommand under a group, can only be used under a group

## Options

- variable max/max length - applies to varargs (length)

### These options also apply to the vararg values

- min/max length - applies to text
- min/max value - applies to integer (value), number (value), date (value), time (value), datetime (value), attachment (size)
- channel_types - limits the type of channels you can pass
- required permissions - the permissions the mentioned user require, users who dont have this permissions wont appear in the search filter
- required roles - same as required permissions but for roles instead
- autocomplete - used for bot side autocomplete, only works for text, integer, number

## Cooldowns

Commands can have a cooldown built-in which has a `limit`, `length` and `bucket`

`limit`: how many can be done in `length` of time
`length`: how long the cooldown lasts
`bucket`: what the cooldown applies to, server, channel, member, etc

# Config

A panel inside server setting which bot devs can configure so servers can edit settings with a good ui.
config will act as a mini rest api where revolt will, post and get the config from the bot and to the bot
editing what entries which exist will be done via the api and take a list of entry types and configs

# Buttons

7 x 5 grid of buttons, can have either an emoji, text or both.

## Spec

![[Interactions lifecycles.canvas]]

## Interactions

```rust

#[serde(tag="type")]
enum SlashCommandOption {
    Text {
        name: String
        value: String,
        required: bool
    },
    Integer {
        name: String,
        value: String,
        required: bool
    },

    Subcommand {
        name: String,
        options: Vec<SlashCommandOption>
    }
}

/*
/mycommand foo: `bar`
[
    {
        "type": "Text",
        "name": "foo",
        "value": "bar"
    }
]

/mycommand foo: `bar` baz: `bah`

/group foo: `bar`
[
    {
        "type": "Text",
        "name": "foo",
        "value": "bar"
    },
    {
        "type": "Text",
        "name": "baz",
        "value": "bah"
    }
]

/group subcommand foo: `bar`
[
    {
        "type": "Subcommand",
        "options": [
            {
                "type": "Text",
                "name": "foo",
                "value": "bar"
            },
        ]
    }
]
*/

#[serde(tag="type")]
enum InnerInteraction {
	SlashCommand {
		server: Server,
		channel: Channel,
		author: User
		token: String,

		name: String,
		options: Vec<SlashCommandOption>
	},
	Button {
		...
	},
	MessageContextMenu {
		...
	}
	MemberContextMenu {
		...
	}
}

struct Interaction {
	id: String,
	custom_id: String,

	#[serde(flatten)]
	inner: InnerInteraction
}
```


## Configuration

```rust

struct RadioButton {
    id: String,
    name: String,
    description: Option<String>,
    enabled: bool,

    #[serde(flatten)]
    option: InnerConfigOption
}

struct ConfigOption {
    id: String,
    name: String,
    description: Option<String>
}

#[serde(tag = "type")]
enum InnerConfigOption {
    Text {
        default: Option<String>,
        value: Option<String>
    },
    Integer {
        default: Option<String>,
        value: Option<String>
    }
    Role {
        default: Option<String>,
        value: Option<String>
    },
    Channel {
        default: Option<String>,
        value: Option<String>
        channel_types: Vec<ChannelType>
    },
    Dropdown {
        options: Vec<Option>,
        value: Option<ConfigOption>,
        default: ConfigOption
    },
    Switch {
        value: bool,
        default: Option<bool>
    },
    RadioButtons {
        options: Vec<RadioButton>,
        default: String
    },
    ColourPicker {
        transparency: bool,
        value: u32,
        default: u32
    }
}

struct Category {
    id: String,
    name: String,
    options: Vec<ConfigOption>
}

struct ConfigurationType {
    Server,
    Channel
}

struct Configuration {
    description: Option<String>,
    r#type: ConfigurationType
    categories: Vec<Category>,
    footer: Option<String>
}
```
