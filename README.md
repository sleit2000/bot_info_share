# Discord Activity Bot

A feature-rich Discord bot designed to track and encourage user activity through a points-based system with seasonal leaderboards and AI-powered content validation.

## Features

### Activity Tracking System
- **Message Scoring**: Awards points based on message length (30 chars = 1 point, max 15 points per message)
- **Daily Limits**: Prevents spam with daily point caps (100 points/day)
- **Point Eligibility Interval**: 30-second minimum between messages to earn points (does not delete messages)
- **Quality Bonuses**: Extra points for questions (+2) and code blocks (+3)
- **AI Validation**: OpenAI integration with caching to detect and prevent meaningless messages (>20 chars)

### Seasonal Leaderboards
- **Monthly Seasons**: Automatically resets at the start of each month
- **Final Score Calculation**: Combines points with active days for balanced ranking (points × active_days/30)
- **Historical Data**: Stores all past seasons for reference
- **Hall of Fame**: Tracks all-time top contributors across seasons
- **Per-Guild Data**: Separate tracking for each Discord server

### Commands
- `!rank` / `!top10` - Shows current season leaderboard (5s cooldown)
- `!stats [user]` - Displays detailed user statistics (5s cooldown)
- `!seasonhistory [season_id]` - Views past season results (5s cooldown)
- `!hof` - Shows all-time top contributors (5s cooldown)
- `!help` - Shows available commands based on user role
- `!ping` - Bot response time test
- `!status` - Bot status information
- `!serverinfo` - Server information (admin only)

### Moderation Tools
- **Role-based permission system** (User → Bot Moderator → Bot Administrator → Server Creator → Bot Owner)
- **Warning system** (`!warn @user`)
- **Timeout management** (`!unmute @user`, `!mutelist`)
- **Moderator management** (`!addmod @user`, `!deletemod @user`)

### Administration
- **Server information** (`!serverinfo`)
- **Role assignment** (`!addmod`, `!deletemod`)
- **Owner-only server control** (`!servers`, `!leave <server_id>`, `!userlist`, `!delete_user <guild_id> @user`)

## Technical Details

### Architecture
- Modular design with separate command categories and core components
- Per-guild JSON file storage with SQLite database for settings
- Rotating log system with automatic cleanup
- Configurable through environment variables
- Graceful shutdown handling with proper resource cleanup

### Core Technologies
- Python 3.8+
- discord.py library
- JSON for activity data storage
- SQLite for guild settings and permissions
- OpenAI integration with caching (optional, for AI validation)
- Schedule library for automated tasks

### Data Storage
- User activity data: `data/user_activity.json` (per-guild structure)
- Season history: `data/seasons.json`
- Guild settings: SQLite database per guild in `data/guilds/`
- AI response cache: `openai_cache.db`
- Logs: Rotating logs in `logs/` directory and per-guild logs

## Activity Tracking Configuration

The activity tracker uses these default settings (configurable in code):
- `MIN_CHARS`: 5 - Minimum characters for a message to earn points
- `POINTS_PER_CHAR`: 30 - Characters needed per point
- `MAX_POINTS_PER_MSG`: 15 - Maximum points per single message
- `DAILY_POINT_CAP`: 100 - Maximum points per day per user
- `MIN_MESSAGE_INTERVAL`: 30 - Minimum seconds between messages to be eligible for points (does not delete messages)

## Permission System

The bot uses a hierarchical role system:
1. **User** - Basic commands: help, rank, stats, seasonhistory, hof
2. **Bot Moderator** - User commands + ping, warn, status
3. **Bot Administrator** - Moderator commands + serverinfo, addmod, deletemod, unmute, mutelist
4. **Server Creator** - Administrator commands
5. **Bot Owner** - All commands including server management (servers, leave, userlist, delete_user)

## AI Integration

For messages longer than 20 characters, the bot uses OpenAI to validate message quality:
- **🔍** - Validation in progress
- **✅** - Meaningful messages receive points and get a ✅ reaction
- **🚫** - Meaningless messages are flagged with 🚫 reaction and receive no points
- **Caching** - AI responses are cached in SQLite database to reduce API calls
- **Fallback** - If AI validation fails, messages are processed normally

The AI module uses a sophisticated caching system with configurable TTL to minimize API costs while maintaining accuracy.

## Message Reactions

The bot uses various emoji reactions to provide feedback:
- **⭐** - High-quality message (5+ points)
- **➕** - Regular message (1+ points)
- **🔍** - AI validation in progress
- **✅** - Message validated as meaningful by AI
- **🚫** - Message flagged as meaningless by AI

## Error Handling

The bot includes comprehensive error handling:
- Graceful shutdown with proper resource cleanup
- Automatic log rotation and cleanup
- Per-guild error isolation
- Fallback mechanisms for AI validation failures
- Command cooldowns to prevent command spam

## Future Improvements
- Web dashboard for statistics visualization
- Customizable point thresholds and bonuses per server
- Integration with additional AI services
- Automated rewards system for top contributors
- Enhanced moderation tools with automated actions
- Export functionality for statistics
- Multi-language support

This bot is designed to encourage quality discussions while providing administrators with comprehensive tools to manage their communities effectively. The modular architecture makes it easy to extend with new features while maintaining stability and performance.
