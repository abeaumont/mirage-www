LIBS   = -pkgs lwt.syntax,js_of_ocaml,js_of_ocaml.syntax,js_of_ocaml.log,c3,uri,re.str,rrd,bigarray
PKGS   = js_of_ocaml lwt c3 uri xapi-rrd re rpc
SYNTAX = -tags "syntax(camlp4o),annot,bin_annot,strict_sequence,principal,thread"

SYNTAX += -tag-line "<static*.*>: -syntax(camlp4o)"

FLAGS  = -cflag -g -lflags -g,-linkpkg

BUILD  = ocamlbuild -use-ocamlfind $(LIBS) $(SYNTAX) $(FLAGS)
OPAM   = opam

export PKG_CONFIG_PATH=$(shell opam config var prefix)/lib/pkgconfig

export OPAMVERBOSE=1
export OPAMYES=1

.PHONY: all depend clean build main.byte install
all:: build install

install: build
	mkdir -p ../files/js/stats
	cp main.min.js ../files/js/stats/main.js
	# workaround djs55/ocaml-c3:12
	$(OPAM) remove c3

depend::
	$(OPAM) install $(PKGS) --verbose

main.byte:
	$(BUILD) main.byte

build: main.min.js

main.min.js: main.js
	jsoo_minify main.js

main.js: main.byte
	js_of_ocaml +weak.js main.byte

clean::
	ocamlbuild -clean

-include Makefile.user
