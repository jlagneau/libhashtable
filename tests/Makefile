#******************************************************************************#
#                                                                              #
#                                                         :::      ::::::::    #
#    Makefile                                           :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: jlagneau <jlagneau@student.42.fr>          +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2013/11/21 08:29:58 by jlagneau          #+#    #+#              #
#    Updated: 2015/12/04 08:46:16 by jlagneau         ###   ########.fr        #
#                                                                              #
#******************************************************************************#

# Variables
NAME      = test

# Information variables
ISGIT     := $(shell find . -name ".git" -type d)
HASTAGS   := $(shell git tag)

ifneq (, $(strip $(ISGIT)))
ifneq (, $(strip $(HASTAGS)))
VER      := $(shell git describe --tags `git rev-list --tags --max-count=1`)
else
VER      := $(shell git rev-parse --short HEAD)
endif
GDATE    := $(shell git show -s --format="%ci" HEAD)
endif

# Directories
FT_PATH  = ../libft/
FT_INC   = $(FT_PATH)include/

PRJ_PATH = ../
PRJ_INC  = ../include/

SRCS_PATH = src/
HEAD_PATH = include/

OBJS_PATH = .obj/
DEPS_PATH = .dep/

# Exec
RM        = rm -rf

# Flags
CFLAGS    = -Wall -Wextra -Werror -pedantic
CPPFLAGS  = -I$(HEAD_PATH) -I$(FT_INC) -I$(PRJ_INC)
LDFLAGS   = -L$(FT_PATH) -L$(PRJ_PATH)
LDLIBS    =
DEPSFLAGS = -MMD -MF"$(DEPS_PATH)$(notdir $(@:.o=.d))"

# Files
SRCS     := $(shell find src -type f)

DEPS      = $(addprefix $(DEPS_PATH), $(notdir $(SRCS:.c=.d)))
OBJS      = $(addprefix $(OBJS_PATH), $(notdir $(SRCS:.c=.o)))

DEB_OBJS  = $(OBJS:.o=_debug.o)
DEB_DEPS  = $(addprefix $(DEPS_PATH), $(notdir $(DEB_OBJS:.o=.d)))

# Phony
.PHONY: all clean fclean norme re redebug

# Rules
$(NAME): CFLAGS += -O3
$(NAME): LDLIBS += -lft -lhashtable
$(NAME): $(OBJS)
	$(CC) $^ $(LDFLAGS) $(LDLIBS) -o $@

debug: CFLAGS += -g3
debug: LDLIBS += -lft_debug -lhashtable_debug
debug: $(DEB_OBJS)
	$(CC) $^ $(LDFLAGS) $(LDLIBS) -o $@

$(OBJS_PATH)%.o: $(SRCS_PATH)%.c
	@mkdir -p $(OBJS_PATH) $(DEPS_PATH)
	$(CC) $(CFLAGS) $(CPPFLAGS) $(DEPSFLAGS) -c $< -o $@

$(OBJS_PATH)%_debug.o: $(SRCS_PATH)%.c
	@mkdir -p $(OBJS_PATH) $(DEPS_PATH)
	$(CC) $(CFLAGS) $(CPPFLAGS) $(DEPSFLAGS) -c $< -o $@

norme:
	@norminette ./**/*.{h,c}

all: $(NAME)

clean:
	$(RM) $(OBJS_PATH) $(DEPS_PATH)
	@make -C $(FT_PATH) clean

fclean:
	$(RM) $(OBJS_PATH) $(DEPS_PATH)
	$(RM) $(NAME) debug
	@make -C $(FT_PATH) fclean

re: fclean all

redebug: fclean debug

-include $(DEPS)
-include $(DEB_DEPS)
