---
title: "Observer"
weight: -10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: true
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---
# Observer

Observer pattern sends out a notification to every object subscribed to it.

## Examples:

### Detecting object interaction:
- Player stood on the button:
	- Door was subscribed to the event so it opened.
	
In this example, event is emmited in the physics check:

{{< tabs >}}
{{% tab "GDScript" %}}
Button:
```gdscript
signal pressed

func _on_body_entered(body: Node3D):
	if body.is_in_group(&"player"):
		pressed.emit()
```

Door:
```gdscript
func _on_button_pressed():
	open()
```

``pressed`` signal has been connected to door script through editor, and automatically generated a function name.
{{% /tab %}}
{{% tab "Unity" %}}
Button:
```csharp
public UnityEvent OnPressed;

private void OnTriggerEnter(Collider other)
{
	if (other.GetComponent<ButtonPusher>()){
		OnPressed.Invoke();
	}
}
```

Door:
```csharp
public void Activate()
{
	Open()
}
```
``Activate`` method has been connected to ``OnPressed`` Event in the Inspector.
{{% /tab %}}
{{< /tabs >}}

### Detecting value change:
- Player's HP has changed, so ``on_health_changed(value: float)`` event have been emmited:
	- UI Health Bar was subscribed to the event, so it changed the visuals to the new value.
	- Animation Player was subscribed to the event, so it played the hurt animation.
	- Invincibility Timer was subscribed to the event, so it changed Health Component's state to Invincible.

In this example, event is emmited in the setter:

{{< tabs >}}
{{% tab "GDScript" %}}
Setter emitting the signal:
```gdscript
signal health_changed(value: float)

var health: float:
	set(value):
		health = value
		health_changed.emit(health)
```

Signal ``health_changed`` can be connected to built-in methods, like `TextureProgressBar`'s `set_value`.

If you need to connect it to method that requires no parameters (`Timer.start()`), you can unbind a parameter in-editor.

If you need to connect it to a method that requires a parameter not provided by the signal (`AnimationPlayer.play(name: StringName)`), you can bind an additional parameter in-editor.
{{% /tab %}}
{{% tab "Unity" %}}
```csharp
public UnityEvent<float> OnHealthChanged;

private float _health;
public float Health
	{
		get { return _health; }
		set {
			_health = value;
			OnHealthChanged.Invoke(_health);
		}
	}
```
{{% /tab %}}
{{< /tabs >}}

## Terminology

Object sending notifications is called Publisher, and object recieving notifications - Subscriber.


## Properties

It provides several benefits:
- Decoupling:
	- Publisher isn't aware of objects subscribed to it, only aware of subscriber methods.
	- Subscriber doesn't have to store reference to a Publisher after subscribing.
- Interfacing:
	- Only specific method is shared.

Compared to Global Events, Observer provides access on a lower scope. 

## Using

Modern engines have observer pattern built in, like Godot's ``signal`` and Unity's ``UnityEvent``.

They also allow connecting events to methods in editor, instead of doing everything through code. This way the connection is being saved to the Scene.

This helps to reduce amount of referencing between nodes, cutting amount of exported variables. 