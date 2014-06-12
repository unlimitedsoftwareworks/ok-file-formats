# Requires ImageMagick
#
CC = clang -Wall -O3 -std=c99

BIN = bin
GEN = gen
PNG_SUITE = PngSuite
INCLUDES = -I .. 
FILES = *.c ../*.c
OUT_NAME = ok_file_formats_test

# Get all png files
PNG_SUITE_FILES := $(wildcard $(PNG_SUITE)/*.png)
RGBA_FILES := $(patsubst $(PNG_SUITE)/%.png, $(GEN)/%.rgba, $(PNG_SUITE_FILES))

test: gen build
	$(BIN)/$(OUT_NAME)
	
build:  
	$(CC) -o $(BIN)/$(OUT_NAME) $(INCLUDES) $(FILES)

# Convert png files to raw RGBA8888 format, ignoring the gAMA chunk.
# The "-intensity Average" setting prevents ImageMagick from 
# color-converting grayscale files.
# There will be warnings for files "oi9n0g16.png" and "oi9n2c16.png" but
# they will convert successfully.
$(GEN)/%.rgba: $(PNG_SUITE)/%.png
	convert -define png:exclude-chunk=gAMA $< temp.png
	convert -depth 8 -intensity Average temp.png $@
	@rm temp.png
	
gen: init $(RGBA_FILES)

init:
	@mkdir -p $(BIN)
	@mkdir -p $(GEN)
	
clean:
	rm -rf $(GEN)
	rm -f $(BIN)/$(OUT_NAME)