#! /usr/bin/make

CHANNELD_HEADERS :=			\
	channeld/gen_full_channel_error_names.h	\
	channeld/channeld_wiregen.h		\
	channeld/channeld_htlc.h		\
	channeld/commit_tx.h			\
	channeld/full_channel.h			\
	channeld/full_channel_error.h		\
	channeld/watchtower.h

CHANNELD_SRC := channeld/channeld.c	\
	channeld/commit_tx.c			\
	channeld/full_channel.c			\
	channeld/channeld_wiregen.c		\
	channeld/watchtower.c

CHANNELD_OBJS := $(CHANNELD_SRC:.c=.o)
$(CHANNELD_OBJS): $(CHANNELD_HEADERS)

# Make sure these depend on everything.
ALL_C_SOURCES += $(CHANNELD_SRC)
ALL_C_HEADERS += $(CHANNELD_HEADERS)
ALL_PROGRAMS += lightningd/lightning_channeld

# Here's what lightningd depends on
LIGHTNINGD_CONTROL_HEADERS += channeld/channeld_wiregen.h
LIGHTNINGD_CONTROL_OBJS += channeld/channeld_wiregen.o

# Common source we use.
CHANNELD_COMMON_OBJS :=				\
	common/amount.o				\
	common/base32.o				\
	common/bigsize.o			\
	common/bip32.o				\
	common/blinding.o			\
	common/channel_config.o			\
	common/channel_id.o			\
	common/crypto_state.o			\
	common/crypto_sync.o			\
	common/cryptomsg.o			\
	common/daemon.o				\
	common/daemon_conn.o			\
	common/derive_basepoints.o		\
	common/dev_disconnect.o			\
	common/ecdh_hsmd.o			\
	common/features.o			\
	common/fee_states.o			\
	common/status_wiregen.o			\
	common/peer_status_wiregen.o		\
	common/gossip_rcvd_filter.o		\
	common/gossip_store.o			\
	common/hmac.o				\
	common/htlc_state.o			\
	common/htlc_trim.o			\
	common/htlc_tx.o			\
	common/htlc_wire.o			\
	common/initial_channel.o		\
	common/initial_commit_tx.o		\
	common/keyset.o				\
	common/key_derive.o			\
	common/memleak.o			\
	common/msg_queue.o			\
	common/node_id.o			\
	common/onion.o				\
	common/onionreply.o			\
	common/peer_billboard.o			\
	common/peer_failed.o			\
	common/penalty_base.o			\
	common/per_peer_state.o			\
	common/permute_tx.o			\
	common/ping.o				\
	common/pseudorand.o			\
	common/read_peer_msg.o			\
	common/setup.o				\
	common/sphinx.o				\
	common/status.o				\
	common/status_wire.o			\
	common/subdaemon.o			\
	common/timeout.o			\
	common/type_to_string.o			\
	common/utils.o				\
	common/utxo.o				\
	common/version.o			\
	common/wire_error.o			\
	common/wireaddr.o			\
	gossipd/gossipd_peerd_wiregen.o		\
	lightningd/gossip_msg.o			\
	wire/fromwire.o				\
	wire/towire.o

ifeq ($(EXPERIMENTAL_FEATURES),1)
CHANNELD_COMMON_OBJS += common/psbt_open.o
endif

channeld/gen_full_channel_error_names.h: channeld/full_channel_error.h ccan/ccan/cdump/tools/cdump-enumstr
	ccan/ccan/cdump/tools/cdump-enumstr channeld/full_channel_error.h > $@

lightningd/lightning_channeld: $(CHANNELD_OBJS) $(WIRE_ONION_OBJS) $(CHANNELD_COMMON_OBJS) $(WIRE_OBJS) $(BITCOIN_OBJS) $(HSMD_CLIENT_OBJS)

include channeld/test/Makefile
