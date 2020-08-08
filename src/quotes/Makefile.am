
quotedir                = $(exec_prefix)/etc/quotes
nobase_dist_quote_DATA  = anagrams.yaml authors.yaml english.yaml french.yaml funstuff.yaml guilds.yaml

AUTOMAKE_OPTIONS        = gnu
ACLOCAL_AMFLAGS         = -I build
EXTRA_DIST              = Makefile.am french-bug.yaml
                          $(nobase_dist_etc_DATA)

dist-hook:
	@date >DIST_REVISION
	@echo git branch -vv >>DIST_REVISION
	@cp DIST_REVISION $(distdir)/

#endif
