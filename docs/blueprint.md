# AI BOOSTER — Bot specification

**Archetype:** custom

**Voice:** professional and concise — write every user-facing message, button label, error, and empty state in this voice.

Delivers AI-generated trading signals in English and Arabic with risk-managed position suggestions for beginner traders. Supports signal history tracking, performance summaries, and future automated trading integration.

> This is the complete contract for the bot. Implement EVERY entry point, flow, feature, integration, and edge case below. The completeness review checks the bot against this document after each build pass.

## Primary audience

- beginner retail traders
- cryptocurrency traders
- stock market newcomers

## Success criteria

- Users receive localized trading signals with confidence scores
- Signal performance tracking implemented
- Subscription management interface functional
- Future automated trading integration ready

## Entry points

Every feature must be reachable from the bot's command/button surface (button-first; only /start and /help are slash commands).

- **/start** (command, actor: user, command: /start) — Begin onboarding flow
- **New Signal** (button, actor: user, callback: signal:show) — Request latest trading signal
  - inputs: selected language, risk profile
  - outputs: structured signal message
- **My Subscriptions** (button, actor: user, callback: subs:manage) — View and manage subscription tier

## Flows

### onboarding
_Trigger:_ /start

1. Language selection
2. Risk tolerance setup
3. Exchange preference selection

_Data touched:_ user profile

### signal_delivery
_Trigger:_ signal:show

1. Generate localized signal content
2. Display with action buttons
3. Track signal status

_Data touched:_ signal, user preferences

### subscription_management
_Trigger:_ subs:manage

1. Show current tier
2. Display upgrade options
3. Process payment confirmation

_Data touched:_ subscription plan

## Data entities

Durable data (must survive a restart) uses the toolkit's persistent store, never in-memory maps.

- **user_profile** _(retention: persistent)_ — User preferences and metadata
  - fields: language, risk_tolerance, preferred_exchanges, subscription_status
- **trading_signal** _(retention: persistent)_ — Generated market signal with parameters
  - fields: symbol, direction, entry_price, stop_loss, take_profit, confidence_score, rationale_en, rationale_ar
- **signal_performance** _(retention: persistent)_ — Historical performance tracking
  - fields: signal_id, execution_price, exit_price, outcome

## Integrations

- **Telegram** (required) — Bot API messaging
- **AI Signal Engine** (required) — Generate trading rationales
Call external APIs against their real contract (correct endpoints, ids, params); credentials from env. Do not fake responses.

## Owner controls

- Manage subscription tiers
- Configure signal broadcast schedules
- View performance analytics dashboard

## Notifications

- Signal delivery via direct message
- Channel broadcast for premium signals
- Email alerts for critical price movements

## Permissions & privacy

- User data stored securely with encryption
- Clear disclaimer about non-financial-advice
- Compliance with exchange API terms of service

## Edge cases

- Failed signal delivery retries
- User cancels mid-transaction
- Exchange API rate limiting

## Required tests

- End-to-end onboarding flow with language selection
- Signal delivery with all action buttons
- Subscription upgrade/downgrade scenarios

## Assumptions

- Payment integration handled by platform
- Exchange APIs will be implemented later
- All content includes educational disclaimer
