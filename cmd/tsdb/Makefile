# Copyright 2018 The Prometheus Authors
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

STATICCHECK_IGNORE =
TSDB_BIN = "./tsdb"
NUM_METRICS ?= 1000
PROJECT_DIR = "../.."
TEST_DATA_DIR ?= "$(PROJECT_DIR)/testdata"
TEST_DATASET ?= "20kseries.json"
BENCH_DIR ?= "./benchout"

include ../../Makefile.common

# Since tsdb client yet has no tests we can at least try to run benchmark
test: bench

build:
	@$(GO) build .

bench: build
	@echo ">> running benchmark"
	@$(TSDB_BIN) bench write --metrics=$(NUM_METRICS) $(TEST_DATA_DIR)/$(TEST_DATASET)

bench-vizualization: bench
	@command -v dot >/dev/null 2>&1 || echo "You need to have Graphviz installed to genrate pprof vizualization."
	@$(GO) tool pprof -svg ./tsdb $(BENCH_DIR)/cpu.prof > $(BENCH_DIR)/cpuprof.svg
	@$(GO) tool pprof --inuse_space -svg ./tsdb $(BENCH_DIR)/mem.prof > $(BENCH_DIR)/memprof.inuse.svg
	@$(GO) tool pprof --alloc_space -svg ./tsdb $(BENCH_DIR)/mem.prof > $(BENCH_DIR)/memprof.alloc.svg
	@$(GO) tool pprof -svg ./tsdb $(BENCH_DIR)/block.prof > $(BENCH_DIR)/blockprof.svg
	@$(GO) tool pprof -svg ./tsdb $(BENCH_DIR)/mutex.prof > $(BENCH_DIR)/mutexprof.svg

clean:
	rm -f $(TSDB_BIN)
	rm -rf $(BENCH_DIR)

.PHONY: clean test
