# Complete Guide to Using `keyd` for Key Remapping on Arch Linux

## Step 1: Install `keyd`

1. **Install from the official repo**:
   ```bash
   sudo pacman -S keyd
   ```

2. **Alternatively, install from AUR**:
   ```bash
   git clone https://aur.archlinux.org/keyd.git
   cd keyd
   makepkg -si
   ```

## Step 2: Enable and Start the `keyd` Service

1. **Start and enable `keyd` to run on boot**:
   ```bash
   sudo systemctl enable keyd
   sudo systemctl start keyd
   ```

2. **Check if `keyd` is running**:
   ```bash
   sudo systemctl status keyd
   ```
   It should show: `Active: active (running)`.

## Step 3: Create a Key Remapping Configuration

1. **Create or edit the configuration file for key remapping**:
   ```bash
   sudo nano /etc/keyd/default.conf
   ```

2. **Add the following content to remap `Right Alt` to `;`**:
   ```ini
   [ids]

   *

   [main]

   rightalt = semicolon
   ```

   This configuration will remap the `Right Alt` key to semicolon (`;`).

## Step 4: Apply the Configuration

To apply the changes made in the configuration file, restart the `keyd` service:
```bash
sudo systemctl restart keyd
```

## Step 5: Make Sure It's Loaded on Boot

Ensure that the configuration is applied on system startup:
```bash
sudo systemctl enable keyd
```

## Step 6: Test the Remap

1. Open a text editor or terminal.
2. Press `Right Alt` → You should get `;`.
3. Press `Shift + Right Alt` → You should get `:` (if you've also mapped `Shift + Right Alt` in your configuration).

## Step 7: Debug Mode (Optional)

You can also run `keyd` in debug mode to see what keys it's detecting:
```bash
sudo keyd -m
```

## Additional Configuration Options

You can add more key remaps or custom layers in the configuration file. Here's an example:

```ini
[ids]
*

[main]
rightalt = semicolon
shift+rightalt = colon
```

This will remap `Right Alt` to `;` and `Shift + Right Alt` to `:`.

## Step 8: Reboot Your System (Optional)

After you've made all your changes, you may want to reboot to ensure the remaps persist and apply correctly:
```bash
reboot
```

---

That's it! You’ve now set up `keyd` to remap your `Right Alt` key and any other keys you wish. You can adjust the `default.conf` to remap other keys, create different layers, or even use custom key combinations.