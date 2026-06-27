CODEC_URL = https://github.com/phoboslab/qoi/archive/97bacc86a9c4abf5a2d452102dc26546c4c670b9.tar.gz

CODEC_DIR = node_modules/qoi
CODEC_BUILD_DIR:= $(CODEC_DIR)/build
ENVIRONMENT = worker

OUT_JS = enc/qoi_enc.js dec/qoi_dec.js
OUT_WASM := $(OUT_JS:.js=.wasm)

.PHONY: all clean

all: $(OUT_JS)

$(filter enc/%,$(OUT_JS)): enc/qoi_enc.o
$(filter dec/%,$(OUT_JS)): dec/qoi_dec.o

# ALL .js FILES
$(OUT_JS):
	$(LD) \
		$(LDFLAGS) \
		--bind \
		-s ENVIRONMENT=$(ENVIRONMENT) \
		-s EXPORT_ES6=1 \
		-o $@ \
		$+

# ALL .o FILES
%.o: %.cpp $(CODEC_DIR)
	$(CXX) -c \
		$(CXXFLAGS) \
		-I $(CODEC_DIR) \
		-o $@ \
		$<

# CREATE DIRECTORY
$(CODEC_DIR):
	mkdir -p $(CODEC_DIR)
	curl -sL $(CODEC_URL) | tar xz --strip 1 -C $(CODEC_DIR)

clean:
	$(RM) $(OUT_JS) $(OUT_WASM)
	$(MAKE) -C $(CODEC_DIR) clean
