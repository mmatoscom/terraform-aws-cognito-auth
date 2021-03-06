# Copyright (c) 2018-2019 Martin Donath <martin.donath@squidfunk.com>

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to
# deal in the Software without restriction, including without limitation the
# rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
# sell copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
# IN THE SOFTWARE.

include ../../../share/env/Makefile
include ../../../share/lambda/Makefile

# -----------------------------------------------------------------------------
# Target hooks
# -----------------------------------------------------------------------------

# Generate JSON schema files from event typings
dist-pre: \
$(patsubst %.ts, %.json, $(shell find src/common/events/*/* -name "*.ts"))

# Include schemas in distribution files
dist-post: $(subst src, dist, $(shell find src/common/* -name "*.json"))

# -----------------------------------------------------------------------------
# Targets
# -----------------------------------------------------------------------------

# Copied JSON schema file
dist/%.json: src/%.json
	@ mkdir -p dist/$(dir $*)
	cp $< $@

# JSON schema file generated from request typings
src/%.json: src/%.ts
	$(shell npm bin)/typescript-json-schema \
		--required \
		--noExtraProps \
		$< __JSON__ > $@

# -----------------------------------------------------------------------------
# Rules
# -----------------------------------------------------------------------------

# Start development server
start: node_modules
	@ [ ! -f ${ROOT}/terraform.tfstate ] || make --silent .env
	@ NODE_ENV=development ${NPM_BIN}/nodemon \
		--quiet --watch src --watch tools --ext json,ts \
		--exec ${NPM_BIN}/ts-node \
			-r tsconfig-paths/register \
			--project tools/tsconfig.json \
			--files tools/server
