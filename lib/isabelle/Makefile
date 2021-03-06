THYS = Sail_instr_kinds.thy Sail_values.thy Sail_operators.thy \
  Sail_operators_mwords.thy Sail_operators_bitlists.thy \
  State_monad.thy State.thy Prompt_monad.thy Prompt.thy
EXTRA_THYS = State_monad_lemmas.thy State_lemmas.thy Prompt_monad_lemmas.thy \
  Sail_operators_mwords_lemmas.thy Hoare.thy

RISCV_DIR = ../../riscv

.PHONY: all heap-img clean

all: thys

thys: $(THYS)

heap-img: thys $(EXTRA_THYS) ROOT
	@echo '*** To build a heap image with the Sail library, please'
	@echo '*** add this directory to your ROOTS file'
	@echo '*** (e.g. $$HOME/.isabelle/Isabelle<version>/ROOTS)'
	@echo '*** and add the isabelle binary to your PATH.'
	isabelle build -b Sail

manual: heap-img manual/Manual.thy manual/ROOT manual/document/root.tex
	cp output/document/session_graph.pdf manual/document/Sail_session_graph.pdf
	make -C $(RISCV_DIR) Riscv_duopod.thy
	isabelle build -d manual Sail-Manual

Sail_instr_kinds.thy: ../../src/lem_interp/sail_instr_kinds.lem
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

Sail_values.thy: ../../src/gen_lib/sail_values.lem Sail_instr_kinds.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

Sail_operators.thy: ../../src/gen_lib/sail_operators.lem Sail_values.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

Sail_operators_mwords.thy: ../../src/gen_lib/sail_operators_mwords.lem Sail_operators.thy Prompt.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

Sail_operators_bitlists.thy: ../../src/gen_lib/sail_operators_bitlists.lem Sail_operators.thy Prompt.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

Prompt_monad.thy: ../../src/gen_lib/prompt_monad.lem Sail_values.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

Prompt.thy: ../../src/gen_lib/prompt.lem Prompt_monad.thy Prompt_monad_lemmas.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

State_monad.thy: ../../src/gen_lib/state_monad.lem Sail_values.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

State.thy: ../../src/gen_lib/state.lem Prompt.thy State_monad.thy State_monad_lemmas.thy
	lem -isa -outdir . -auxiliary_level none -lib ../../src/lem_interp -lib ../../src/gen_lib $<

clean:
	-rm $(THYS)
