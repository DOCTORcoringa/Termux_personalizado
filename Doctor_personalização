#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
Doctor Personalização - Painel de customização do Termux
Criado por Doctor Coringa Lunático 🌙 - 2025

Painel com banner ASCII colorido, prompt customizado e persistência no shell.
"""

import os
import json
import time
import pyfiglet
from rich.console import Console
from rich.panel import Panel
from rich.prompt import Prompt, IntPrompt, Confirm
from rich.progress import Progress, BarColumn, TextColumn, TimeElapsedColumn
from rich.align import Align
from rich.text import Text

console = Console()
CONFIG_PATH = os.path.expanduser("~/.doctor_personalizacao_config.json")

STYLES = [
    "Simples: nome$",
    "Colorido: nome com cor e $",
    "Estilo médico: Doctor@nome>",
    "Estilo avançado: [Doctor nome] #",
    "Sem estilo: Apenas $"
]

COLORS = [
    "red",
    "green",
    "yellow",
    "blue",
    "magenta",
    "cyan",
    "white"
]

BANNER_STYLES = [
    "standard",
    "3-d",
    "banner3-D",
    "starwars",
    "slant"
]

def clear_screen():
    os.system('clear' if os.name == 'posix' else 'cls')

def load_config():
    if os.path.isfile(CONFIG_PATH):
        try:
            with open(CONFIG_PATH, "r", encoding="utf-8") as f:
                return json.load(f)
        except Exception:
            pass
    return {
        "name": "",
        "banner_style": 0,
        "banner_color": "green",
        "style": 0,
        "color": "green",
        "password_enabled": False,
        "password": ""
    }

def save_config(cfg):
    with open(CONFIG_PATH, "w", encoding="utf-8") as f:
        json.dump(cfg, f, ensure_ascii=False, indent=4)

def loading(message: str, duration=3):
    with Progress(
        TextColumn("[progress.description]{task.description}"),
        BarColumn(),
        TimeElapsedColumn(),
        transient=True,
        console=console
    ) as progress:
        task = progress.add_task(message, total=100)
        for _ in range(100):
            progress.update(task, advance=1)
            time.sleep(duration/100)

def show_credits_panel():
    credit_text = "[dim]Doctor Personalização - Criado por Doctor Coringa Lunático 🌙🩺 - 2025[/dim]"
    panel = Panel(credit_text, style="dim", padding=(0,1))
    console.print(Align.center(panel))

def termcolor_code(color_name):
    colors = {
        "black": 30,
        "red": 31,
        "green": 32,
        "yellow": 33,
        "blue": 34,
        "magenta": 35,
        "cyan": 36,
        "white": 37,
    }
    return colors.get(color_name.lower(), 32)

def generate_bashrc(cfg):
    bashrc_path = os.path.expanduser("~/.bashrc")
    banner_name = cfg.get("name") or "Doctor"
    banner_style = BANNER_STYLES[cfg.get("banner_style", 0)]
    banner_color = cfg.get("banner_color", "green")
    prompt_style = cfg.get("style", 0)
    prompt_color = cfg.get("color", "green")

    banner_command = f"""
figlet -f {banner_style} "{banner_name}" | GREP_COLOR='01;{termcolor_code(banner_color)}' grep --color=always '.'
"""

    PS1_map = {
        0: f'\\[\\e[1;{termcolor_code(prompt_color)}m\\]{banner_name}\\[\\e[0m\\]$ ',
        1: f'\\[\\e[1;{termcolor_code(prompt_color)}m\\]{banner_name}\\[\\e[0m\\]\\[\\e[1;{termcolor_code(prompt_color)}m\\]$\\[\\e[0m\\] ',
        2: f'\\[\\e[1;32m\\]Doctor@\\[\\e[1;{termcolor_code(prompt_color)}m\\]{banner_name}\\[\\e[0m\\]> ',
        3: f'\\[\\e[1;34m\\][Doctor \\[\\e[1;{termcolor_code(prompt_color)}m\\]{banner_name}\\[\\e[1;34m\\]] #\\[\\e[0m\\] ',
        4: f'\\[\\e[0m\\]$ '
    }
    prompt_cmd = PS1_map.get(prompt_style, PS1_map[0])

    bashrc_content = f"""
# Doctor Personalização custom banner and prompt start

clear
{banner_command}
export PS1="{prompt_cmd}"

# Doctor Personalização custom banner and prompt end
"""

    try:
        with open(bashrc_path, "w", encoding="utf-8") as f:
            f.write(bashrc_content)
        console.print(f"[green]Arquivo ~/.bashrc atualizado com sucesso para aplicar banner e prompt personalizados.[/green]")
    except Exception as e:
        console.print(f"[red]Erro ao atualizar ~/.bashrc: {e}[/red]")

def painel():
    cfg = load_config()

    clear_screen()
    welcome_msg = """
👨‍⚕️🩺 Doctor Personalização - Painel de Customização do Termux 🩺👨‍⚕️

Criação: Doctor Coringa Lunático 🌙🩺
Ano: 2025

Este painel permite personalizar o seu banner ASCII colorido e estilo do prompt.

⚠️ As configurações são salvas e aplicadas automaticamente na próxima abertura do Termux!

Pressione Enter para continuar...
"""
    console.print(Panel(welcome_msg.strip(), style="cyan", padding=(1,2)))
    input()

    loading("Carregando painel de personalização...", 2)

    while True:
        clear_screen()
        if cfg.get("name"):
            config_status = (
                f"[bold white]Configurações atuais[/bold white]\n"
                f"- Nome/banner: [yellow]{cfg['name']}[/yellow]\n"
                f"- Estilo do banner ASCII: [yellow]{BANNER_STYLES[cfg['banner_style']]}[/yellow]\n"
                f"- Cor do banner: [yellow]{cfg['banner_color'].capitalize()}[/yellow]\n"
                f"- Estilo do prompt: [yellow]{STYLES[cfg['style']]}[/yellow]\n"
                f"- Cor do prompt: [yellow]{cfg['color'].capitalize()}[/yellow]\n"
                f"- Senha ativada: [yellow]{'Sim' if cfg['password_enabled'] else 'Não'}[/yellow]"
            )
        else:
            config_status = "[bold white]Nenhuma configuração salva ainda.[/bold white]"

        painel_text = f"""
{config_status}

[bold]Menu principal:[/bold]
1. Alterar nome/banner
2. Alterar estilo do banner ASCII
3. Alterar cor do banner
4. Alterar estilo do prompt
5. Alterar cor do prompt
6. Ativar / Desativar senha
7. Visualizar painel
8. Voltar para configurações padrão
9. Sair e salvar
"""
        console.print(Panel(Align.center(painel_text.strip(), vertical="middle"),
                            title="Doctor Personalização", border_style="green", padding=(1,2),
                            subtitle="[bold cyan]Doctor Coringa Lunático 🌙🩺 - 2025[/bold cyan]"))
        show_credits_panel()

        try:
            escolha = IntPrompt.ask("Escolha uma opção", choices=[str(i) for i in range(1,10)])
        except KeyboardInterrupt:
            console.print("\nSaindo sem salvar...")
            break

        # Sempre mostrar a barra de loading após cada escolha válida (antes de aplicar)
        def show_loading(msg):
            loading(f"{msg}...", 2)

        if escolha == 1:
            input_name(cfg)
            show_loading("Aplicando nome")
        elif escolha == 2:
            input_banner_style(cfg)
            show_loading("Aplicando estilo do banner ASCII")
        elif escolha == 3:
            input_banner_color(cfg)
            show_loading("Aplicando cor do banner")
        elif escolha == 4:
            input_style(cfg)
            show_loading("Aplicando estilo do prompt")
        elif escolha == 5:
            input_color(cfg)
            show_loading("Aplicando cor do prompt")
        elif escolha == 6:
            input_password(cfg)
            show_loading("Configurando senha")
        elif escolha == 7:
            show_panel_mockup(cfg)
        elif escolha == 8:
            reset_config(cfg)
            show_loading("Resetando configurações")
        elif escolha == 9:
            save_config(cfg)
            generate_bashrc(cfg)
            console.print("\n[green]Configurações salvas e shell personalizado aplicado! Saindo...[/green]")
            break

def input_name(cfg):
    while True:
        clear_screen()
        title = "Banner / Nome"
        prompt_text = "[italic]Digite seu nome para o banner personalizado:[/italic]"
        console.print(Panel(Align.center(f"[bold green]Doctor Personalização[/bold green]\n\n{prompt_text}", vertical="middle"),
                            title=title, border_style="blue", padding=(1,2)))
        show_credits_panel()
        name = Prompt.ask("Nome", default=cfg.get("name", "Doctor"))
        if not name.replace(" ","").isalpha():
            console.print("[red]Digite apenas letras e espaços.[/red]")
            time.sleep(2)
            continue
        cfg["name"] = name
        if not Confirm.ask("Deseja alterar o nome novamente?", default=False):
            break

def input_banner_style(cfg):
    while True:
        clear_screen()
        title = "Estilo do Banner ASCII"
        console.print(Panel(Align.center("[bold green]Doctor Personalização[/bold green]\n\n[italic]Escolha o estilo do banner ASCII:[/italic]", vertical="middle"),
                            title=title, border_style="blue", padding=(1,2)))
        show_credits_panel()
        for idx, style_name in enumerate(BANNER_STYLES):
            console.print(f"  [cyan]{idx}[/cyan]: {style_name}")
        choice = IntPrompt.ask(f"Digite o número da opção desejada (0 a {len(BANNER_STYLES)-1})",
                              choices=[str(i) for i in range(len(BANNER_STYLES))])
        cfg["banner_style"] = int(choice)
        if not Confirm.ask("Deseja alterar o estilo do banner novamente?", default=False):
            break

def input_banner_color(cfg):
    while True:
        clear_screen()
        title = "Cor do Banner"
        console.print(Panel(Align.center("[bold green]Doctor Personalização[/bold green]\n\n[italic]Escolha a cor do banner:[/italic]", vertical="middle"),
                            title=title, border_style="blue", padding=(1,2)))
        show_credits_panel()
        for idx, color_text in enumerate(COLORS):
            console.print(f"  [cyan]{idx}[/cyan]: [{color_text}]{color_text.capitalize()}[/]")
        choice = IntPrompt.ask(f"Digite o número da opção desejada (0 a {len(COLORS)-1})",
                              choices=[str(i) for i in range(len(COLORS))])
        cfg["banner_color"] = COLORS[int(choice)]
        if not Confirm.ask("Deseja alterar a cor do banner novamente?", default=False):
            break

def input_style(cfg):
    while True:
        clear_screen()
        title = "Estilo do Prompt"
        console.print(Panel(Align.center("[bold green]Doctor Personalização[/bold green]\n\n[italic]Escolha o estilo do prompt:[/italic]", vertical="middle"),
                            title=title, border_style="blue", padding=(1,2)))
        show_credits_panel()
        for idx, style_text in enumerate(STYLES):
            console.print(f"  [cyan]{idx}[/cyan]: {style_text}")
        choice = IntPrompt.ask(f"Digite o número da opção desejada (0 a {len(STYLES)-1})",
                              choices=[str(i) for i in range(len(STYLES))])
        cfg["style"] = int(choice)
        if not Confirm.ask("Deseja alterar o estilo do prompt novamente?", default=False):
            break

def input_color(cfg):
    while True:
        clear_screen()
        title = "Cor do Prompt"
        console.print(Panel(Align.center("[bold green]Doctor Personalização[/bold green]\n\n[italic]Escolha a cor do prompt:[/italic]", vertical="middle"),
                            title=title, border_style="blue", padding=(1,2)))
        show_credits_panel()
        for idx, color_text in enumerate(COLORS):
            console.print(f"  [cyan]{idx}[/cyan]: [{color_text}]{color_text.capitalize()}[/]")
        choice = IntPrompt.ask(f"Digite o número da opção desejada (0 a {len(COLORS)-1})",
                              choices=[str(i) for i in range(len(COLORS))])
        cfg["color"] = COLORS[int(choice)]
        if not Confirm.ask("Deseja alterar a cor do prompt novamente?", default=False):
            break

def input_password(cfg):
    while True:
        clear_screen()
        title = "Senha de Proteção"
        console.print(Panel(Align.center("[bold green]Doctor Personalização[/bold green]\n\n[italic]Ativar ou desativar senha para proteger o Termux?[/italic]", vertical="middle"),
                            title=title, border_style="blue", padding=(1,2)))
        show_credits_panel()
        enabled = Confirm.ask("Ativar senha?", default=cfg.get("password_enabled", False))
        cfg["password_enabled"] = enabled
        if enabled:
            pwd1 = Prompt.ask("Digite a senha", password=True)
            pwd2 = Prompt.ask("Confirme a senha", password=True)
            if pwd1 == pwd2:
                cfg["password"] = pwd1
                loading("Senha configurada...", 2)
            else:
                console.print("[red]Senhas não conferem. A senha foi desativada.[/red]")
                cfg["password_enabled"] = False
                cfg["password"] = ""
                loading("Cancelando configuração de senha...", 2)
        else:
            cfg["password"] = ""
            loading("Senha removida...", 2)
        if not Confirm.ask("Deseja alterar a senha novamente?", default=False):
            break

def reset_config(cfg):
    clear_screen()
    console.print(Panel("[bold red]Tem certeza que deseja voltar para configurações padrão?[/bold red]\nIsto removerá todas as personalizações!",
                        title="Atenção", border_style="red", padding=(1,2)))
    show_credits_panel()
    confirm = Confirm.ask("Confirmar reset?", default=False)
    if confirm:
        cfg.clear()
        cfg.update({
            "name": "",
            "banner_style": 0,
            "banner_color": "green",
            "style": 0,
            "color": "green",
            "password_enabled": False,
            "password": ""
        })
        save_config(cfg)
        generate_bashrc(cfg)
        loading("Configurações resetadas para padrão...", 3)
        console.print("[green]Configurações restauradas ao padrão do Termux.[/green]")
        time.sleep(2)

def show_panel_mockup(cfg):
    clear_screen()
    title = "Visualização do Painel"
    show_credits_panel()
    banner_text = pyfiglet.figlet_format(cfg["name"] or "Doctor", font=BANNER_STYLES[cfg["banner_style"]])
    banner_colored = Text(banner_text, style=cfg["banner_color"])
    prompt_example = generate_prompt_preview(cfg)
    console.print(Panel(banner_colored, title="Banner:", border_style=cfg["banner_color"]))
    console.print(Panel(prompt_example, title="Exemplo do Prompt", border_style=cfg["color"]))
    console.print("\nPressione Enter para voltar ao menu...")
    input()

def generate_prompt_preview(cfg):
    from rich.style import Style
    from rich.text import Text

    style_idx = cfg.get("style", 0)
    name = cfg.get("name") or "Doctor"
    color = cfg.get("color", "green")

    name_styled = Text(name, style=Style(color=color,bold=True))
    prompt = Text()

    if style_idx == 0:
        prompt.append(name_styled)
        prompt.append("$ ")
    elif style_idx == 1:
        prompt.append(name_styled)
        prompt.append(Text("$ ", style=Style(color=color,bold=True)))
    elif style_idx == 2:
        prompt.append(Text("Doctor@", style=Style(color="green",bold=True)))
        prompt.append(name_styled)
        prompt.append("> ")
    elif style_idx == 3:
        prompt.append(Text("[Doctor ", style=Style(color="blue",bold=True)))
        prompt.append(name_styled)
        prompt.append(Text("] # ", style=Style(color="blue",bold=True)))
    elif style_idx == 4:
        prompt.append(Text("$ ", style=Style(color="white")))
    else:
        prompt.append(Text("$ ", style=Style(color="white")))

    return prompt

def main():
    clear_screen()
    painel()

if __name__ == "__main__":
    main()
