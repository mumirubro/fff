# Replit Configuration for TOJI CHK Telegram Bot

## Overview

TOJI CHK is a Telegram bot built using the python-telegram-bot library. The bot provides card checking, BIN lookup, and utility tools with a registration-based user system. Currently named "Ayaka" in documentation, it implements a command-based interface with user registration requirements and hierarchical command menus.

**Core Purpose:** A feature-rich Telegram bot for card checking operations, BIN lookups, and account verification tools with admin controls and user management.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Application Structure

**Bot Framework:**
- Built on `python-telegram-bot` (v20.7+) using async/await patterns
- Command handler architecture for processing user interactions
- Inline keyboard-based navigation system for commands and features

**User Management System:**
- User registration required before accessing bot features
- User data persisted in `users.json` file (flat-file storage)
- User records include: user_id, username, and registration timestamp
- Single registration per user ID enforced

**Command Architecture:**
- `/start` - Initial entry point prompting registration
- `/register` - User registration handler
- `/cmd` - Main command menu with auto-playing video (toji.mp4) and inline keyboard navigation
- Hierarchical menu system with categories:
  1. Admin usernames and commands
  2. Tools
  3. Gates
  4. Account checker

**Performance Optimizations:**
- Video caching: toji.mp4 is uploaded once and reused via file_id to reduce response time
- Safe message editing: Custom helper function handles both text and video messages to prevent callback errors
- Fallback strategy: When editing video messages fails, bot deletes and sends new text message

**Data Storage:**
- JSON-based file storage for user data
- No database system currently implemented
- User records stored with ISO 8601 timestamps

**Bot Configuration:**
- Environment-based token management via `.env` file
- Bot token should be stored securely (not hardcoded)
- Modular command structure for easy feature expansion

### Design Decisions

**Why JSON File Storage:**
- Simple deployment without database dependencies
- Suitable for small-to-medium user base
- Easy backup and portability
- **Trade-off:** Not scalable for large concurrent user loads; consider migrating to SQLite or PostgreSQL for production

**Why Registration Requirement:**
- User tracking and access control
- Prevents anonymous abuse of card checking features
- Enables future premium/tier-based features
- **Trade-off:** Adds friction to user onboarding

**Why Inline Keyboards:**
- Better UX than text-based command navigation
- Reduces command memorization burden
- Enables visual hierarchy for feature discovery
- **Trade-off:** Requires more complex state management

## External Dependencies

### Third-Party Libraries

1. **python-telegram-bot (>=20.7)**
   - Core bot framework
   - Handles Telegram API interactions
   - Provides async command handlers and callback query handling

2. **requests (>=2.31.0)**
   - HTTP client for external API calls
   - Used for BIN lookup and card checking integrations
   - Handles third-party payment gateway verification

3. **python-dotenv (>=1.0.0)**
   - Environment variable management
   - Securely loads bot token and configuration
   - Separates secrets from codebase

### External Services

1. **Telegram Bot API**
   - Primary communication platform
   - Bot token required from @BotFather
   - Webhook or polling-based message retrieval

2. **Card Checking APIs** (Not yet implemented)
   - Third-party payment gateway integrations
   - BIN lookup services
   - Card validation endpoints
   - **Note:** Specific providers to be determined based on requirements

3. **Account Verification Services** (Not yet implemented)
   - External account checker integrations
   - API credentials will be required

### Deployment Environment

- **Target Platform:** Linux VPS (Ubuntu/Debian)
- **Python Version:** 3.11 or higher
- **Process Management:** Consider implementing systemd service or supervisor for production
- **No containerization currently configured** (Docker could be added for easier deployment)