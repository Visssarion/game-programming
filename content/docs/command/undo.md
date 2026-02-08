---
title: "Undo"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---
# Undo
Undo is built upon the [Command pattern]( {{< relref "docs/command/_index" >}})

Main difference, is that now the class holds an `undo` function, that completes an opposite operation of `do`.

This is very useful when you want your player to cancel their previous action, like in a Puzzle game.

> Think about all the times Ctrl+Z saved you!

If you store all your previous Commands in a [Stack]( {{< relref "docs/data_structures/stack" >}}), you will create Action History.
This way you can allow your player to Undo any amount of Commands.

## Using

{{< tabs >}}
{{% tab "GDScript" %}}
Base class:
```gdscript
@abstract
class_name PlayerAction
extends RefCounted

@abstract func do(player: Player) # Main action

@abstract func undo(player: Player) # Reverse action

func can_perform(player: Player): # You can add more methods.
	return true
```

Push
```gdscript
extends PlayerAction

var _pushed_object = null


func do(player: Player):
	var front_object = player.get_front_object()
	front_object.move(Vector3.FORWARD)
	_pushed_object = front_object # Storing object in case we need to reference it in `undo`.

func undo(player: Player):
	_pushed_object.move(Vector3.BACKWARD) # Doing the opposite of what we did in `do`.

func can_perform(player: Player):
	var front_object = player.get_front_object()
	return front_object != null 
	

```

{{% /tab %}}
{{% tab "Unity" %}}
TODO
{{% /tab %}}
{{< /tabs >}}