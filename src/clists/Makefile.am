
clistsdir                = $(exec_prefix)/etc/clists
nobase_dist_clists_DATA  = \
                           capitals.yaml \
                           currency.yaml \
                           gec-fips14.yaml \
                           genc-gec-as.yaml genc-gec-ge.yaml \
                           geonames.yaml \
                           iso-15924.yaml \
                           iso-3166-1.yaml iso-3166-2.yaml iso-3166-3.yaml \
                           iso-4217.yaml \
                           iso-639-2.yaml iso-639-3.yaml iso-639-5.yaml \
                           months.yaml months-fr.yaml \
                           time-period.yaml \
                           timezone.yaml \
                           weekday.yaml weekdays-fr.yaml

AUTOMAKE_OPTIONS         = gnu
ACLOCAL_AMFLAGS          = -I build
EXTRA_DIST               = Makefile.am $(nobase_dist_clists_DATA)

