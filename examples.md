## Demo Application ##
> The JNativeHook library ships with a demo applciation containing all avaiable features.  http://code.google.com/p/jnativehook/source/browse/trunk/src/java/org/jnativehook/example/NativeHookDemo.java


## Global Keyboard Listener ##
```
import org.jnativehook.GlobalScreen;
import org.jnativehook.NativeHookException;
import org.jnativehook.keyboard.NativeKeyEvent;
import org.jnativehook.keyboard.NativeKeyListener;

public class GlobalKeyListenerExample implements NativeKeyListener {
	public void nativeKeyPressed(NativeKeyEvent e) {
		System.out.println("Key Pressed: " + NativeKeyEvent.getKeyText(e.getKeyCode()));

		if (e.getKeyCode() == NativeKeyEvent.VK_ESCAPE) {
			GlobalScreen.unregisterNativeHook();
		}
	}

	public void nativeKeyReleased(NativeKeyEvent e) {
		System.out.println("Key Released: " + NativeKeyEvent.getKeyText(e.getKeyCode()));
	}

	public void nativeKeyTyped(NativeKeyEvent e) {
		System.out.println("Key Typed: " + e.getKeyText(e.getKeyCode()));
	}

	public static void main(String[] args) {
		try {
			GlobalScreen.registerNativeHook();
		}
		catch (NativeHookException ex) {
			System.err.println("There was a problem registering the native hook.");
			System.err.println(ex.getMessage());

			System.exit(1);
		}

		//Construct the example object and initialze native hook.
		GlobalScreen.getInstance().addNativeKeyListener(new GlobalKeyListenerExample());
	}
}
```

## Global Mouse Listener ##
```
import org.jnativehook.GlobalScreen;
import org.jnativehook.NativeHookException;
import org.jnativehook.mouse.NativeMouseEvent;
import org.jnativehook.mouse.NativeMouseInputListener;

public class GlobalMouseListenerExample implements NativeMouseInputListener {
	public void nativeMouseClicked(NativeMouseEvent e) {
		System.out.println("Mosue Clicked: " + e.getClickCount());
	}

	public void nativeMousePressed(NativeMouseEvent e) {
		System.out.println("Mosue Pressed: " + e.getButton());
	}

	public void nativeMouseReleased(NativeMouseEvent e) {
		System.out.println("Mosue Released: " + e.getButton());
	}

	public void nativeMouseMoved(NativeMouseEvent e) {
		System.out.println("Mosue Moved: " + e.getX() + ", " + e.getY());
	}

	public void nativeMouseDragged(NativeMouseEvent e) {
		System.out.println("Mosue Dragged: " + e.getX() + ", " + e.getY());
	}

	public static void main(String[] args) {
		try {
			GlobalScreen.registerNativeHook();
		}
		catch (NativeHookException ex) {
			System.err.println("There was a problem registering the native hook.");
			System.err.println(ex.getMessage());

			System.exit(1);
		}

		//Construct the example object.
		GlobalMouseListenerExample example = new GlobalMouseListenerExample();

		//Add the appropriate listeners for the example object.
		GlobalScreen.getInstance().addNativeMouseListener(example);
		GlobalScreen.getInstance().addNativeMouseMotionListener(example);
	}
}
```

## Global Mouse Wheel Listener ##
```
import org.jnativehook.GlobalScreen;
import org.jnativehook.NativeHookException;
import org.jnativehook.mouse.NativeMouseWheelEvent;
import org.jnativehook.mouse.NativeMouseWheelListener;

public class GlobalMouseWheelListenerExample implements NativeMouseWheelListener {
	public void nativeMouseWheelMoved(NativeMouseWheelEvent e) {
		System.out.println("Mosue Wheel Moved: " + e.getWheelRotation());
	}

	public static void main(String[] args) {
		try {
			GlobalScreen.registerNativeHook();
		}
		catch (NativeHookException ex) {
			System.err.println("There was a problem registering the native hook.");
			System.err.println(ex.getMessage());
			ex.printStackTrace();

			System.exit(1);
		}

		//Construct the example object and initialze native hook.
		GlobalScreen.getInstance().addNativeMouseWheelListener(new GlobalMouseWheelListenerExample());
	}
}
```

## Working with Swing ##
JNativeHook does **NOT** operate on the event dispatch thread.  Because Swing components are not thread safe, you **MUST** wrap access to Swing components using the [SwingUtilities.invokeLater()](http://docs.oracle.com/javase/1.5.0/docs/api/javax/swing/SwingUtilities.html#invokeLater(java.lang.Runnable)) or [EventQueue.invokeLater()](http://docs.oracle.com/javase/1.5.0/docs/api/java/awt/EventQueue.html#invokeLater(java.lang.Runnable)) methods.
```
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.SwingUtilities;
import javax.swing.WindowConstants;
import org.jnativehook.GlobalScreen;
import org.jnativehook.NativeHookException;
import org.jnativehook.keyboard.NativeKeyEvent;
import org.jnativehook.keyboard.NativeKeyListener;

public class SwingExample extends JFrame implements NativeKeyListener, WindowListener {
	public SwingExample() {
		setTitle("JNativeHook Swing Example");
		setSize(300, 150);
		setDefaultCloseOperation(WindowConstants.DISPOSE_ON_CLOSE);
		addWindowListener(this);
		setVisible(true);
	}

	public void windowOpened(WindowEvent e) {
		//Initialze native hook.
		try {
			GlobalScreen.registerNativeHook();
		}
		catch (NativeHookException ex) {
			System.err.println("There was a problem registering the native hook.");
			System.err.println(ex.getMessage());
			ex.printStackTrace();

			System.exit(1);
		}

		GlobalScreen.getInstance().addNativeKeyListener(this);
	}

	public void windowClosed(WindowEvent e) {
		//Clean up the native hook.
		GlobalScreen.unregisterNativeHook();
		System.runFinalization();
		System.exit(0);
	}

	public void windowClosing(WindowEvent e) { /* Unimplemented */ }
	public void windowIconified(WindowEvent e) { /* Unimplemented */ }
	public void windowDeiconified(WindowEvent e) { /* Unimplemented */ }
	public void windowActivated(WindowEvent e) { /* Unimplemented */ }
	public void windowDeactivated(WindowEvent e) { /* Unimplemented */ }

	public void nativeKeyReleased(NativeKeyEvent e) {
		if (e.getKeyCode() == NativeKeyEvent.VK_SPACE) {
			SwingUtilities.invokeLater(new Runnable() {
				public void run() {
					JOptionPane.showMessageDialog(null, "This will run on Swing's Event Dispatch Thread.");
				}
			});
		}
	}

	public void nativeKeyPressed(NativeKeyEvent e) { /* Unimplemented */ }
	public void nativeKeyTyped(NativeKeyEvent e) { /* Unimplemented */ }

	public static void main(String[] args) {
		 SwingUtilities.invokeLater(new Runnable() {
			public void run() {
				new SwingExample();
			}
		});
	}
}
```