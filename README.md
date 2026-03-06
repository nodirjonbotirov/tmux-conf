# ==============================================================================
# JONATHON'S UPDATED TMUX CONFIG
# ==============================================================================

# --- Asosiy Sozlamalar ---
set -s default-terminal "screen-256color"
set -g prefix C-b
set -s escape-time 50
set -g history-limit 30000
set -g display-time 3000
setw -g aggressive-resize on

# --- Clipboard va Sichqoncha (AVTO-COPY INTEGRATSIYASI) ---
set -g mouse on
setw -g mode-keys vi

# Sichqoncha bilan belgilab, tugmani qo'yib yuborganda avtomatik nusxalash
# Bu qism matnni ham tmux buferiga, ham tizim clipboardiga (xclip) yuboradi
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard"
bind-key -T copy-mode MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard"

# M-w orqali nusxalash (tizim clipboardiga)
bind-key -n -Tcopy-mode M-w send -X copy-pipe-and-cancel "xclip -i -sel p -f | xclip -i -sel c" \; display-message "Copied to Clipboard"

# C-y orqali tizim clipboardidan tmuxga tashlash (Paste)
bind-key -n C-y run "xclip -o -sel clipboard | tmux load-buffer - ; tmux paste-buffer"

# --- Panel va Oynalarni boshqarish ---
bind r source-file ~/.tmux.conf \; display "~/.tmux.conf reloaded!"

bind Up select-pane -U
bind k select-pane -U
bind Down select-pane -D
bind j select-pane -D
bind Left select-pane -L
bind h select-pane -L
bind Right select-pane -R
bind l select-pane -R

bind = select-layout -E
bind + select-layout tiled

# Panel raqamlari orqali o'tish
bind 1 select-pane -t 1
bind 2 select-pane -t 2
bind 3 select-pane -t 3
bind 4 select-pane -t 4
bind 5 select-pane -t 5
bind 6 select-pane -t 6

# Windowlarni tanlash (M-1, M-2...)
bind M-1 select-window -t :=1
bind M-2 select-window -t :=2
bind M-3 select-window -t :=3
bind M-4 select-window -t :=4
bind M-5 select-window -t :=5
bind M-6 select-window -t :=6
bind M-7 select-window -t :=7
bind M-8 select-window -t :=8
bind M-9 select-window -t :=9

bind M-c last-window
bind -r t kill-pane \; set -u pane-active-border-style
bind T confirm-before "kill-pane -a"
bind X confirm-before "kill-window -a"

set -g base-index 1
setw -g pane-base-index 1

# Oynalarni boshqarish (Tab va M-x/v)
bind -r M-x previous-window
bind -r M-v next-window
bind -r Tab next-window

# Split-window (Joriy papkada ochish)
bind -r v split-window -p 50 -h -c "#{pane_current_path}" \; set -u pane-active-border-style
bind -r '\' split-window -p 50 -h -c "#{pane_current_path}" \; set -u pane-active-border-style
bind -r h split-window -p 50 -c "#{pane_current_path}" \; set -u pane-active-border-style
bind -r - split-window -p 50 -c "#{pane_current_path}" \; set -u pane-active-border-style
bind c new-window

# --- Vizual va Status Bar ---
set -g status-interval 1
set -g status-bg default
set -g status-fg default
set -g status-left "[#S#I#P:#{pane_height}x#{pane_width}] "
set -g status-right '#(~/.local/bin/ssh_ip)#(~/.local/bin/speed_net)<#[reverse,bold]%H:%M#[default] %Y-%m-%d %A>'
set -g status-right-length 200
set -g status-left-length 100

set -g pane-border-status top
set -g pane-border-format "#[reverse,bold]#(echo '#')#{pane_index}: #{s|$HOME|~|:pane_current_path}#[default] #(gitmux -q -fmt tmux #{pane_current_path})#[default]"

# Zoom funksiyasi
bind z if-shell "[[ #{window_panes} -eq 1 || #{window_zoomed_flag} -eq 1 ]]" "resize-pane -Z ; set-option pane-active-border-style fg=red,bold" "resize-pane -Z ; set-option pane-active-border-style fg=green,bold,italics"
bind M-z if-shell "[[ #{window_panes} -eq 1 || #{window_zoomed_flag} -eq 1 ]" "resize-pane -Z; set-option pane-active-border-style fg=red,bold" "last-pane ; resize-pane -Z; set-option pane-active-border-style fg=green,bold,italics"

setw -g window-status-current-style fg=red,bright
setw -g monitor-activity on
set -g visual-activity on
setw -g window-status-activity-style bold,reverse,underscore

# --- Boshqa qulayliklar ---
bind M-s copy-mode
bind-key -n -Tcopy-mode M-a send-keys -X page-up
bind-key -n -Tcopy-mode M-z send-keys -X page-down

# Panellarni o'lchamini o'zgartirish
bind -r M-Up    resize-pane -U 2
bind -r M-Down  resize-pane -D 2
bind -r M-Left  resize-pane -L 2
bind -r M-Right resize-pane -R 2
bind-key -r IC   resize-pane -U 5
bind-key -r DC   resize-pane -D 5
bind-key -r Home resize-pane -L 5
bind-key -r End  resize-pane -R 5

# Miscellaneous
unbind-key C-z
unbind-key C-d
