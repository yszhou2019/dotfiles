.PHONY: help

help:
	@echo "Usage: make [target] or make [number]"
	@echo ""
	@echo "Available targets:"
	@grep -E '^[a-zA-Z_-]+:.*?## .*?$$' $(MAKEFILE_LIST) | \
	awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m \033[1;34m%s\033[0m\n", NR". " $$1, $$2}'

install:  ## install all packages from Brewfile
	@echo "Installing packages from Brewfile..." && \
	brew bundle --file=Brewfile

update:  ## update Homebrew and all packages
	@echo "Updating Homebrew..." && \
	brew update && \
	echo "Upgrading packages..." && \
	brew upgrade && \
	brew upgrade --cask && \
	echo "Update complete!"

cleanup:  ## cleanup old versions and caches
	@echo "Cleaning up old versions..." && \
	brew cleanup && \
	brew autoremove && \
	echo "Cleanup complete!"

dump:  ## dump currently installed packages to Brewfile
	@echo "Dumping installed packages to Brewfile..." && \
	brew bundle dump --file=Brewfile --force && \
	echo "Brewfile updated!"

check:  ## check if all Brewfile packages are installed
	@echo "Checking Brewfile packages..." && \
	brew bundle check --file=Brewfile

list:  ## list all installed packages
	@echo "=== Formulae ===" && \
	brew list && \
	echo "" && \
	echo "=== Casks ===" && \
	brew list --cask

.DEFAULT:
	@if ! echo "$@" | grep -qE '^[0-9]+$$'; then \
		echo "make: *** No rule to make target '$@'. Stop." >&2; \
		exit 1; \
	fi

	@TARGET_NAME=$$(grep -E '^[a-zA-Z_-]+:.*?## .*?$$' $(MAKEFILE_LIST) | awk -v num="$@" 'NR==num {split($$0, a, ":"); print a[1]}'); \
	\
	if [ -z "$$TARGET_NAME" ]; then \
		echo "make: *** Invalid selection: $@. Run 'make help' to see available options." >&2; \
		exit 1; \
	else \
		echo "--- Running command: \033[36mmake $$TARGET_NAME\033[0m ---"; \
		$(MAKE) $$TARGET_NAME; \
	fi
