---
name: "wearables_comms"
title: "Wearables Calls and Messages"
description: >-
  Required for every call or text-message request originating on a wearable:
  resolve named recipients from synced device contacts and invoke the
  originating wearable, not a paired phone.
metadata: { "includeInPrompt": true, "devices": ["audio-wearable", "mcu-wearable"] }
---

# Wearables Calls and Messages

## Purpose

Handle call and text-message requests that originate on a wearable. Resolve a
named recipient from that wearable's synced contacts, then invoke the
originating wearable's native communications command. Do not silently hand the
action to a paired phone.

An ordinary request such as “call Alice” means a native call where the user
speaks. This workflow does not cover a request for the assistant to conduct the
conversation itself.

## Select the device

1. Call `device.list` and select the one device marked `is_request_origin:
   true`.
2. Call `device.describe` for that device. Its live command names and argument
   schemas are authoritative.
3. Keep that same device id for contact lookup and the final action. Never
   choose a paired phone merely because it also advertises a compatible
   command. Only an explicit request to use another device overrides this
   default; resolve and describe that device before proceeding.
4. If no request-origin device is identifiable, or the selected device does
   not advertise the requested capability, explain that the action is
   unavailable rather than silently switching devices.

## Resolve a recipient

Skip contact lookup when the user supplied a complete phone number. Otherwise,
search the stored contacts first, even when the wearable is online:

```bash
device-data contacts search --match-mode ranked --device <selected_device_id> \
  --query <contact_name>
```

Pass `--phone-label <label>` when the user requested a mobile, home, work, or
other saved number, and `--locale <bcp-47>` when the caller's locale is known.
Use the returned match evidence and completeness fields; never guess among
contacts or phone numbers. Ask a short clarifying question when the match is
weak or ambiguous.

Only when the stored search cannot resolve the recipient, fall back to the
selected device's live `contacts.search` command if `device.describe`
advertises it. Do not run the live search first or silently move the lookup to
another device.

## Place a call

Select the native call command advertised by the selected wearable. Current
clients publish `wearables.comms.provider.call`; do not assume that name or its
arguments without checking `device.describe`. Call `device.invoke` with the
selected device id and the resolved phone number in the exact advertised
schema.

## Send a message

Require both a resolved recipient and the message text. Select the native SMS
command advertised by the selected wearable. Current clients publish
`wearables.comms.native.sms`; use its exact schema from `device.describe`, then
call `device.invoke` on that device.

Do not substitute a draft or send command from a paired phone. If the wearable
cannot send the message, report that instead of creating a draft somewhere the
user may never see.

## Report the result

- Report success only when `device.invoke` reports success.
- Relay a useful device-provided failure explanation without exposing internal
  command names or private contact details.
- Do not blindly retry a timed-out, interrupted, or uncertain call or send; the
  action may already have happened.
