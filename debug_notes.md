# Debugging v1.5 Issues

1. **"CAMERA ACCESS DENIED" on Netlify:**
   - On iOS Safari, camera permissions are strictly bound to the user's explicit interaction *in that exact moment*. If the `setTimeout(..., 100)` delay inside `startCamera()` is breaking the trusted execution chain, iOS will silently deny the request. We need to remove the delay and invoke `getUserMedia()` instantly on button click.
   - Also, if the user previously blocked camera access for that Netlify URL, it will auto-deny until they go into Safari Settings and re-allow it.

2. **Images not showing in Databank:**
   - If they are older images from previous tests, they might be stored in the old localStorage structure, or the massive data-URIs (base64 strings) might have exceeded the 5MB localStorage limit, causing it to fail to save.
   - Need to verify the `savePhoto()` logic correctly persists to `photoArchive` without hitting quota limits, or add compression.

3. **Fullscreen drops after GPS popup:**
   - When a native browser prompt (like "Allow Location?") overlays the screen on Android/iOS, the browser often forcibly kicks the web app out of Fullscreen mode for security reasons.
   - Need to add an event listener for `fullscreenchange` or a way to easily re-enter fullscreen without having to reboot the app.
