# Parent Process Manipulation via User Input Simulation - .LNK Shortcut

## About
Do you want to spawn a new process under a different parent process to obfuscate your exploitation chain?
Is PPID spoofing to mainstream for you?

Then this PoC might be somewhat interesting to you!

This project demonstrates a method to start processes under a different parent process (`explorer.exe` in this case) by leveraging keyboard shortcuts of `.LNK` files and user input simulation.
Although this technique may briefly expose its activities to the user (as the screen flashes for a split second due to switching windows), it offers an alternative to more conventional methods.

## How It Works
The method involves creating a custom .lnk (shortcut) file that triggers the desired process when a specific hotkey combination (like `[Ctrl]` + `[Shift]` + `B`) is pressed.
This shortcut is registered in the Windows Start Menu and is assigned a hotkey combination.

The script uses the SendInput Windows function to simulate the necessary keyboard input to trigger the hotkey combination and launch the associated process.

Here's an overview of the process:

1. Shortcut Creation: The script creates a shortcut (`ChromeUpdate.lnk`) in the Start Menu directory (`%AppData%\Microsoft\Windows\Start Menu\`) of the current user.
2. Assign Hotkey: The shortcut is assigned a specific hotkey combination, defined in the script (`[Ctrl]` + `[Shift]` + `B` by default).
3. Switch focused window to task bar, to mitigate the capture of the keyboard input by third-party applications. 
4. Simulate Keystrokes: The script uses the `SendInput` function to simulate pressing the hotkey combination, effectively launching the desired process.

The provided PoC is split into two stages. Stage one is the actual parent process manipulation, while stage two is just a simple message box to show that the technique works:


## Showcase

https://github.com/user-attachments/assets/5cad077a-1a8f-4bf9-9a36-1e6ad5b8c9fc


## Note
This code is intended for proof-of-concept or controlled environments only.
It has not been extensively tested for stability or compatibility, so it may not work as expected in all cases.

The choice of `[Ctrl]` + `[Shift]` + `B` is arbitrary and can be changed to any other key combination that suits your needs.

## Modifications
Custom Hotkey: You can modify the hotkey combination by changing the LINK_KEY define at the top of the script. Choose a combination that is unlikely to be pre-registered by Windows or other applications.

Custom Process: The script currently creates a shortcut that executes itself again with different arguments.
You can modify the SetPath and SetArguments sections to specify any other executable or command.

## Limitations
- Not Fully OPSEC safe: The technique requires you to drop an LNK file on disk which might not be the best idea for your use case.
- May Not Work on All Versions: This technique depends on specific behaviors of the Windows shell, which may change between versions or updates.

## Future Improvements
Different Process Control Methods: More sophisticated methods (like named pipes, atom subscribers, or file-based signals) could be integrated for process communication instead of using command-line arguments.
Also, it might be better to delete the dropped LNK file after spawning the second stage process, but I'll leave that up to you.
