# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Setup
- `poetry install` - Install the SDK and its dependencies
- `poetry run pre-commit install` - Set up pre-commit hooks for code quality checks

### Code Quality
- `poetry run black .` - Format code with Black
- `poetry run black --check alpaca/ tests/` - Check code formatting without making changes
- `make lint` - Run linters (equivalent to above black check)

### Testing
- `poetry run pytest` - Run all unit tests
- `make test` - Run unit tests (equivalent to above)

### Documentation
- `./tools/scripts/generate-docs.sh` - Generate Sphinx documentation
- `make generate` - Generate documentation (equivalent to above)

### Makefile Targets
Use `make help` to see all available targets with descriptions.

## Architecture Overview

This is the official Python SDK for Alpaca's APIs, providing access to trading, market data, and broker functionality.

### Module Structure

The codebase is organized into four main modules under `alpaca/`:

1. **`broker/`** - Broker API client for building investment applications
   - `client.py` - BrokerClient for account management
   - `models/` - Account, funding, trading, and other broker-specific models
   - `requests.py` - Request models for broker operations
   - `enums.py` - Broker-specific enumerations

2. **`trading/`** - Trading API client for stock and crypto trading
   - `client.py` - TradingClient for order management and portfolio operations
   - `models.py` - Order, position, and account models
   - `requests.py` - Request models for trading operations
   - `stream.py` - Real-time trading event streaming
   - `enums.py` - Trading-specific enumerations

3. **`data/`** - Market data APIs for stocks, crypto, options, and news
   - `historical/` - Historical data clients (stock, crypto, option, news)
   - `live/` - Real-time data streaming clients
   - `models/` - Data models (bars, quotes, trades, news, etc.)
   - `requests.py` - Request models for data operations
   - `timeframe.py` - Time frame definitions

4. **`common/`** - Shared utilities and base classes
   - `models.py` - Base model classes (`ValidateBaseModel`, `NonEmptyRequest`)
   - `rest.py` - REST client implementation
   - `exceptions.py` - Custom exception classes
   - `types.py` - Common type definitions

### Key Design Patterns

- **Request Models**: All API operations use Pydantic request models that extend `NonEmptyRequest`
- **Validation**: Extensive use of Pydantic for runtime data validation
- **Client Separation**: Separate clients for different APIs and asset classes
- **Response Models**: Structured response models with `.df` property for pandas DataFrame conversion

### Model Conventions

- New models should extend `alpaca.common.models.ValidateBaseModel`
- Request models should extend `alpaca.common.models.NonEmptyRequest`
- Request models provide `to_request_fields()` method for HTTP parameter conversion
- All models use Pydantic v2 for validation

### Code Quality Standards

- Black formatting is enforced via pre-commit hooks
- All code changes trigger automated testing
- Documentation generation is part of the CI pipeline
- The project follows semantic versioning with poetry-dynamic-versioning