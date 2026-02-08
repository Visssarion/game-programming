---
title: "Command"
weight: -1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: true
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---
# Command
Command is a pattern that turns a procedure into an object.

Doing that gives you all the benefits of using an object.
- Storing your object:
	- Tracking action history
	- Queue of procedures that will be executed
- Adding parameters:
	- Having multiple procedures with same code but different values
	- Fetching procedures result even after it has been finished
- Abstraction:
	- Different classes will have different algorithms for the same procedure. This is used in the pattern Strategy.
	- Switching procedures of the same component for a different functionality.
		- When one monster sees player, it get scared, the other one jumps in.

## Using

{{< tabs >}}
{{% tab "GDScript" %}}
Base class that defines main action of the procedure:
```gdscript
@abstract
class_name PlayerAction
extends RefCounted

@abstract func do(player: Player) # Main action

func can_perform(player: Player): # You can add more methods.
	return true
```

Jump
```gdscript
extends PlayerAction

var impulse = 10.0 # Can be changed after this object is created through .new()

func do(player: Player):
	if player.velocity.y < 0:
		player.velocity.y = impulse
	else:
		player.velocity.y += impulse
	
	if not player.is_grounded:
		player.additional_jumps -= 1

func can_perform(player: Player):
	return player.is_grounded or player.additional_jumps > 0
	

```

After a jump input, ``can_perform`` function can be called to check if action is available to be performed.

If it returns ``true``, ``do()`` can be called.

{{% /tab %}}
{{% tab "Unity" %}}
TODO
{{% /tab %}}
{{< /tabs >}}