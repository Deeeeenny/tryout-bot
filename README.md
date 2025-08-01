# Tryout Assignment Discord Bot

A Discord bot for managing tryout assignments in a specific Discord server (Guild ID `1344501419943661608`). The bot handles tryout scoring for three games (MVSD, RIVALS, TSB), assigns division roles based on scores, and includes fun `/cat` and `/dog` commands for fetching cute animal pictures. Built with Discord.js v14 and Node.js, it took less than 2 days to develop!

## Features

- **Guild Restriction**: Operates only in Guild ID `1344501419943661608`. The bot automatically leaves any other server it’s added to.
- **Tryout System**:
  - `/tryout` command (restricted to User ID `1289271473826959464` with `Tryout Host` role) initiates a tryout process for MVSD, RIVALS, or TSB.
  - Users with the `Tryout Host` role can interact with buttons and modals to submit scores, edit submissions, and confirm results.
  - Supports two-stage modal input for scores (e.g., Aim, Game Sense, Juking for RIVALS).
  - Assigns division roles (e.g., `RV Division 1`, `MVSD Guard`) based on total score (0-50).
  - Sends a ping message (`<@userId>, your tryout results for [game] are ready!`) and detailed embed to the `#tryout-logs` channel.
  - Sends a DM to the evaluated user with the same embed, including their profile picture and "Created in 2 Days" footer.
  - Supports role assignment/removal using role IDs for MVSD, RIVALS, and TSB divisions.
- **Fun Commands**:
  - `/cat`: Fetches a random cat picture from The Cat API.
  - `/dog`: Fetches a random dog picture from Dog CEO API.
  - Both commands display images in embeds with a "Created in 2 Days" footer.
- **Security**:
  - `/tryout` restricted to User ID `1289271473826959464` (with `Tryout Host` role).
  - Bot leaves unauthorized guilds and ignores interactions outside the allowed guild.
  - Ephemeral error messages for invalid permissions or guild.
- **Error Handling**:
  - Validates User IDs, scores (0-10), and Perks/No Perks format (e.g., `5-3` for MVSD).
  - Handles missing roles, channels, or API errors gracefully.

## Prerequisites

- **Node.js**: Version 16.9.0 or higher (tested with v24.4.1).
- **Discord Bot Token**: Obtain from the [Discord Developer Portal](https://discord.com/developers/applications).
- **Dependencies**:
  - `discord.js@14`
  - `axios`
- **Discord Server**:
  - Guild ID: `1344501419943661608`
  - Channel: `#tryout-logs`
  - Roles: `Tryout Host` and division roles (e.g., `MVSD Imperial Warlord`, `RV Division 1`, `TSB Division 4`).
- **Role IDs**: Required for `Tryout Host` and division roles (see Configuration).

## Setup

1. **Clone or Create the Project**:
   ```bash
   mkdir tryout-bot
   cd tryout-bot
   npm init -y
   ```
   Save the bot code as `bot.js` in the project directory (e.g., `C:\Users\lamec\OneDrive\Desktop\ez bot took less than 2 days\bot.js`).

2. **Install Dependencies**:
   ```bash
   npm install discord.js@14 axios
   ```
   Verify installation:
   ```bash
   npm list discord.js axios
   ```

3. **Configure Bot Token**:
   - Go to the [Discord Developer Portal](https://discord.com/developers/applications).
   - Create a new application, add a bot, and copy its token.
   - In `bot.js`, replace `'YOUR_NEW_BOT_TOKEN'` with the bot token.

4. **Configure Role IDs**:
   - Enable **Developer Mode** in Discord (User Settings > Appearance > Developer Mode).
   - In Guild ID `1344501419943661608`, right-click each role, select **Copy ID**, and update `bot.js`:
     - **ALLOWED_ROLES**: Replace `'1289271473826959464'` with the `Tryout Host` role ID.
     - **DIVISION_ROLES**: Replace placeholder IDs (`1000000000000000001` to `1000000000000000024`) with actual role IDs for:
       - MVSD: `MVSD Imperial Warlord`, `MVSD Division 1`, ..., `MVSD Division 4`.
       - RIVALS: `RV Imperial Warlord`, `RV Division 1`, ..., `RV Division 4`.
       - TSB: `TSB Imperial Warlord`, `TSB Division 1`, ..., `TSB Division 4`.
     - Comments in `bot.js` indicate where to update role IDs:
       ```javascript
       // Define allowed roles for interactions (replace with actual Tryout Host role ID)
       const ALLOWED_ROLES = ['1289271473826959464']; // Role ID for Tryout Host
       ```
       ```javascript
       // Define division roles with role IDs for MVSD, RIVALS, TSB
       const DIVISION_ROLES = { ... };
       ```

5. **Bot Permissions**:
   - Ensure the bot has `Manage Roles`, `Send Messages`, `Embed Links`, and `Read Messages/View Channels` permissions in Guild ID `1344501419943661608`.
   - Bot’s role must be above all division roles (e.g., `RV Division 1`, `TSB Guard`) in the role hierarchy.
   - Create `#tryout-logs` channel if it doesn’t exist.

6. **Restrict Bot to Guild**:
   - In the Developer Portal, go to **Bot** tab, uncheck **Public Bot**, and save.
   - Go to **OAuth2** > **URL Generator**:
     - Select scopes: `bot`, `applications.commands`.
     - Select permissions: `Send Messages`, `Manage Roles`, `Embed Links`, `Read Messages/View Channels`.
     - Select Guild: `1344501419943661608`.
     - Generate URL:
       ```
       https://discord.com/api/oauth2/authorize?client_id=YOUR_CLIENT_ID&scope=bot+applications.commands&guild_id=1344501419943661608&permissions=268438528
       ```
     - Use this link to invite the bot to the guild.
   - Reset the bot token if old invites exist and update `bot.js`.

7. **Run the Bot**:
   ```bash
   node bot.js
   ```
   - Bot logs in and registers slash commands (`/tryout`, `/cat`, `/dog`) in the guild.

## Usage

### Tryout System
- **Starting a Tryout**:
  - Only User ID `1289271473826959464` (with `Tryout Host` role) can run `/tryout`.
  - Run `/tryout`, select a game (MVSD, RIVALS, TSB) via buttons.
  - Fill out two modals:
    - **First Modal**: User ID and initial scores (e.g., for RIVALS: Aim, Game Sense, Juking).
    - **Second Modal**: Additional scores (e.g., for RIVALS: Cover, Movement) and optional notes.
  - Review the confirmation embed, edit if needed, and confirm.
  - Results:
    - Assigns division roles (e.g., `RV Division 4`) based on total score (0-50).
    - Sends a ping (`<@userId>, your tryout results for [game] are ready!`) and embed to `#tryout-logs`.
    - Sends a DM to the evaluated user with the embed (including their PFP).
    - Submitter gets an ephemeral confirmation.
- **Other Tryout Hosts**:
  - Users with the `Tryout Host` role can interact with buttons and modals (e.g., `tryout_rivals`, `confirm_tryout_rivals`) for existing tryouts but cannot run `/tryout`.

### Fun Commands
- **/cat**: Displays a random cat picture in an embed with a "Created in 2 Days" footer.
- **/dog**: Displays a random dog picture in an embed with a "Created in 2 Days" footer.
- Available to anyone in the guild.

### Restrictions
- **Guild**: Bot only works in Guild ID `1344501419943661608`. Commands in other guilds return: `This bot can only be used in the designated server.` (ephemeral).
- **/tryout**: Restricted to User ID `1289271473826959464` with `Tryout Host` role. Others get: `Only a specific user can use the /tryout command.`.
- **Buttons/Modals**: Require `Tryout Host` role.
- **Unauthorized Guilds**: Bot leaves any guild other than `1344501419943661608`.

## Testing

1. **Verify Setup**:
   - Check `bot.js` for correct token and role IDs.
   - Ensure `#tryout-logs` exists and bot has permissions.
   - Confirm **Public Bot** is disabled and use the guild-specific invite link.

2. **Test `/tryout`**:
   - With User ID `1289271473826959464` (with `Tryout Host` and a division role, e.g., `RV Grand Reaper`):
     - Run `/tryout`, select RIVALS, submit modals (e.g., Aim: 5, Game Sense: 5, Juking: 4, Cover: 5, Movement: 4).
     - Verify `#tryout-logs` shows ping and embed, user gets DM, roles assigned (e.g., `RV Division 4`).
     - Test Edit modal (limited to 5 fields for MVSD).
   - With another user (with `Tryout Host`):
     - Try `/tryout`, expect: `Only a specific user can use the /tryout command.`.
     - Interact with buttons/modals from an existing tryout; they should work.
   - Test role assignment/removal using role IDs.

3. **Test Guild Restriction**:
   - Add bot to another guild; it should leave (check console logs).
   - Run `/tryout` in another guild; expect: `This bot can only be used in the designated server.`.

4. **Test `/cat` and `/dog`**:
   - Run in the allowed guild; verify embeds with images and footer.

## Debugging Tips

- **Role IDs**:
  - Verify IDs in `ALLOWED_ROLES` and `DIVISION_ROLES` match server roles.
  - Check console for role creation errors (bot creates roles if missing with fallback names like `Role_1000000000000000001`).
- **`/tryout` Restriction**:
  - If User ID `1289271473826959464` can’t use `/tryout`, ensure they have `Tryout Host` role.
  - If others can use `/tryout`, check `interaction.user.id !== '1289271473826959464'`.
- **Guild Restriction**:
  - If bot doesn’t leave other guilds, verify `guildCreate` event and permissions.
  - If commands work in other guilds, check `interaction.guildId !== ALLOWED_GUILD_ID`.
- **Other Errors**:
  - Share stack trace for issues.
  - Test APIs: `curl https://api.thecatapi.com/v1/images/search` or `curl https://dog.ceo/api/breeds/image/random`.
  - Verify file path: `C:\Users\lamec\OneDrive\Desktop\ez bot took less than 2 days\bot.js`.

## Notes

- **Role IDs**: Update `ALLOWED_ROLES` and `DIVISION_ROLES` with actual role IDs from your server. Comments in `bot.js` guide where to place them.
- **Invite Security**: Use the guild-specific invite link and keep **Public Bot** disabled to prevent unauthorized invites.
- **Enhancements**: For a two-modal edit system for MVSD or other features, contact the developer.
- **File Path**: Assumes `bot.js` is at `C:\Users\lamec\OneDrive\Desktop\ez bot took less than 2 days\bot.js`. Adjust if different.

For issues or feature requests, share error logs or describe desired changes. Happy tryouting! <3
