#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Hydraulik_Rohre - Extrem modernes PySide6 GUI
Ein elegantes, Apple-ähnliches Design mit sanften Pastelltönen und Animationen
"""

import sys
import math
import random
from typing import Optional, List
from PySide6.QtWidgets import (
    QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout,
    QLabel, QLineEdit, QPushButton, QScrollArea, QFrame, QSizePolicy
)
from PySide6.QtCore import Qt, QPropertyAnimation, QEasingCurve, QTimer, QParallelAnimationGroup, QPoint
from PySide6.QtGui import QFont, QPainter, QColor, QLinearGradient, QPen, QBrush, QPainterPath

from PySide6.QtWidgets import QPushButton
from PySide6.QtGui import QIcon
from PySide6.QtCore import QSize
from PySide6.QtGui import QPixmap


class ElegantButton(QPushButton):
    """Elegante Button-Klasse mit Apple-ähnlichem Design"""
    def __init__(self, text: str, primary: bool = True, parent: Optional[QWidget] = None):
        super().__init__(text, parent)
        self.primary = primary

        # Logo hinzufügen
        self.setIcon(QIcon(r"C:\Users\admin\Desktop\Python lernen\Logo.png"))
        self.setIconSize(QSize(24, 24))

        # Styling je nach primary
        self.setup_style()
        self.setup_animations()

    def setup_style(self):
        if self.primary:
            bg_color = "#2563eb"
        else:
            bg_color = "#e0e0e0"

        self.setStyleSheet(f"""
            QPushButton {{
                font-family: 'SF Pro Display';
                font-size: 16px;
                padding: 8px 16px;
                border-radius: 8px;
                background-color: {bg_color};
                color: white;
            }}
        """)

    def setup_animations(self):
        # Platzhalter für mögliche Hover/Click-Animationen
        pass


    def __init__(self, text: str, primary: bool = True, parent: Optional[QWidget] = None):
        super().__init__(text, parent)
        self.primary = primary
        self.setup_style()
        self.setup_animations()
    
    def setup_style(self):
        """Button-Styling für elegantes, Apple-ähnliches Aussehen"""
        # Moderne, serifenlose Schriftart
        font = QFont("SF Pro Display", 13, QFont.Weight.Medium)
        self.setFont(font)
        
        # Button-Größe und Ausrichtung
        self.setMinimumSize(160, 52)
        self.setSizePolicy(QSizePolicy.Policy.Preferred, QSizePolicy.Policy.Fixed)
        
        if self.primary:
            # Primärer Button - sanfter Blauton mit Schatten
            self.setStyleSheet("""
                QPushButton {
                    background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                        stop:0 #60a5fa, stop:1 #3b82f6);
                    border: none;
                    border-radius: 26px;
                    color: white;
                    padding: 16px 32px;
                    font-weight: 500;
                    font-size: 15px;
                    box-shadow: 0 4px 20px rgba(59, 130, 246, 0.25);
                }
                QPushButton:hover {
                    background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                        stop:0 #93c5fd, stop:1 #60a5fa);
                    box-shadow: 0 6px 25px rgba(59, 130, 246, 0.35);
                    transform: translateY(-1px);
                }
                QPushButton:pressed {
                    background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                        stop:0 #3b82f6, stop:1 #2563eb);
                    box-shadow: 0 2px 10px rgba(59, 130, 246, 0.3);
                    transform: translateY(0px);
                }
            """)
        else:
            # Sekundärer Button - sanfter Grauton
            self.setStyleSheet("""
                QPushButton {
                    background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                        stop:0 #f3f4f6, stop:1 #e5e7eb);
                    border: 1px solid #d1d5db;
                    border-radius: 26px;
                    color: #374151;
                    padding: 16px 32px;
                    font-weight: 500;
                    font-size: 15px;
                    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
                }
                QPushButton:hover {
                    background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                        stop:0 #ffffff, stop:1 #f9fafb);
                    border-color: #9ca3af;
                    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.12);
                    transform: translateY(-1px);
                }
                QPushButton:pressed {
                    background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                        stop:0 #e5e7eb, stop:1 #d1d5db);
                    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
                    transform: translateY(0px);
                }
            """)
    
    def setup_animations(self):
        """Sanfte Hover-Animationen einrichten"""
        self.enterEvent = lambda event: self.animate_hover(True)
        self.leaveEvent = lambda event: self.animate_hover(False)
    
    def animate_hover(self, entering: bool):
        """Sanfte Hover-Animation ausführen"""
        animation = QPropertyAnimation(self, b"geometry")
        animation.setDuration(300)
        animation.setEasingCurve(QEasingCurve.Type.OutCubic)
        
        current_geometry = self.geometry()
        if entering:
            # Button leicht nach oben verschieben
            new_geometry = current_geometry.adjusted(0, -2, 0, -2)
        else:
            # Button zurück zur ursprünglichen Position
            new_geometry = current_geometry.adjusted(0, 2, 0, 2)
        
        animation.setStartValue(current_geometry)
        animation.setEndValue(new_geometry)
        animation.start()


class ElegantLineEdit(QLineEdit):
    """Elegante Eingabefeld-Klasse mit Apple-ähnlichem Design"""
    
    def __init__(self, placeholder: str = "", parent: Optional[QWidget] = None):
        super().__init__(parent)
        self.setup_style(placeholder)
    
    def setup_style(self, placeholder: str):
        """Eingabefeld-Styling für elegantes Aussehen"""
        # Platzhaltertext setzen
        self.setPlaceholderText(placeholder)
        
        # Moderne, serifenlose Schriftart
        font = QFont("SF Pro Text", 14)
        self.setFont(font)
        
        # Größe und Ausrichtung
        self.setMinimumHeight(56)
        self.setSizePolicy(QSizePolicy.Policy.Expanding, QSizePolicy.Policy.Fixed)
        
        # Elegante Farben und Styling mit sanften Schatten
        self.setStyleSheet("""
            QLineEdit {
                background-color: #ffffff;
                border: 2px solid #e5e7eb;
                border-radius: 16px;
                padding: 16px 20px;
                color: #1f2937;
                font-size: 16px;
                font-weight: 400;
                box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
            }
            QLineEdit:focus {
                border-color: #60a5fa;
                background-color: #ffffff;
                box-shadow: 0 0 0 4px rgba(96, 165, 250, 0.1), 0 4px 20px rgba(0, 0, 0, 0.08);
                outline: none;
            }
            QLineEdit:hover {
                border-color: #d1d5db;
                background-color: #fafafa;
                box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            }
        """)


class ElegantLabel(QLabel):
    """Elegante Label-Klasse mit Apple-ähnlichem Design"""
    
    def __init__(self, text: str, parent=None):
        super().__init__(text, parent)
        
        # Logo hinzufügen (zusätzlich zum Text)
        self.setIcon(QIcon("logo.png"))       # Logo aus dem Python-Ordner
        self.setIconSize(QSize(24, 24))       # Größe anpassen
        
        # Restliches Styling
        self.setStyleSheet("""
            QPushButton {
                font-family: 'SF Pro Display';
                font-size: 16px;
                padding: 8px 16px;
                border-radius: 8px;
                background-color: #2563eb;
                color: white;
            }
        """)
    
    def __init__(self, text: str, size: str = "medium", parent: Optional[QWidget] = None):
        super().__init__(text, parent)
        self.size = size
        self.setup_style()
    
    def setup_style(self):
        """Label-Styling für elegantes Aussehen"""
        # Moderne, serifenlose Schriftart je nach Größe
        if self.size == "large":
            font = QFont("SF Pro Display", 20, QFont.Weight.DemiBold)
            color = "#111827"
            padding = "20px 0px"
        elif self.size == "medium":
            font = QFont("SF Pro Display", 16, QFont.Weight.Medium)
            color = "#374151"
            padding = "12px 0px"
        else:  # small
            font = QFont("SF Pro Text", 14, QFont.Weight.Medium)
            color = "#6b7280"
            padding = "8px 0px"
        
        self.setFont(font)
        
        # Elegante Farben und Typografie
        self.setStyleSheet(f"""
            QLabel {{
                color: {color};
                padding: {padding};
                font-weight: 500;
                letter-spacing: -0.025em;
            }}
        """)
        
        # Größe und Ausrichtung
        self.setSizePolicy(QSizePolicy.Policy.Preferred, QSizePolicy.Policy.Fixed)
        self.setMinimumHeight(40)


class InteractiveBackground(QWidget):
    """Interaktiver, animierter Hintergrund mit Partikeln und Formen"""
    
    def __init__(self, parent: Optional[QWidget] = None):
        super().__init__(parent)
        self.setup_particles()
        self.setup_animation_timer()
        self.setMouseTracking(True)  # Mausbewegungen verfolgen
        self.mouse_pos = QPoint(0, 0)  # Aktuelle Mausposition speichern
    
    def setup_particles(self):
        """Partikel und Formen für den Hintergrund einrichten"""
        # Verschiedene Arten von Partikeln erstellen
        self.particles = []
        
        # Kleine, sanfte Kreise
        for i in range(15):
            particle = {
                'type': 'circle',
                'x': random.uniform(0, 1000),
                'y': random.uniform(0, 800),
                'size': random.uniform(2, 8),
                'speed_x': random.uniform(-0.5, 0.5),
                'speed_y': random.uniform(-0.3, 0.3),
                'opacity': random.uniform(0.2, 0.8),  # kleine Kreise
                'color': self.get_random_pastel_color(),
                'rotation': random.uniform(0, 360),
                'rotation_speed': random.uniform(-1, 1)
            }
            self.particles.append(particle)
        
        # Sanfte, fließende Formen
        for i in range(8):
            particle = {
                'type': 'blob',
                'x': random.uniform(0, 1000),
                'y': random.uniform(0, 800),
                'size': random.uniform(20, 60),
                'speed_x': random.uniform(-0.3, 0.3),
                'speed_y': random.uniform(-0.2, 0.2),
                'opacity': random.uniform(0.3, 0.6),
                'color': self.get_random_pastel_color(),
                'rotation': random.uniform(0, 360),
                'rotation_speed': random.uniform(-0.5, 0.5),
                'blob_points': random.randint(6, 10)
            }
            self.particles.append(particle)
        
        # Große, subtile Gradienten
        for i in range(3):
            particle = {
                'type': 'gradient',
                'x': random.uniform(0, 1000),
                'y': random.uniform(0, 800),
                'size': random.uniform(100, 300),
                'speed_x': random.uniform(-0.1, 0.1),
                'speed_y': random.uniform(-0.05, 0.05),
                'opacity': random.uniform(0.1, 0.3),
                'color1': self.get_random_pastel_color(),
                'color2': self.get_random_pastel_color(),
                'rotation': random.uniform(0, 360),
                'rotation_speed': random.uniform(-0.2, 0.2)
            }
            self.particles.append(particle)
    
    def get_random_pastel_color(self):
        """Zufällige, sanfte Pastellfarbe generieren"""
        # Sanfte Pastelltöne in Blau, Grün und Lila
        colors = [
            QColor(147, 197, 253, 200),  # Sanftes Blau
            QColor(167, 243, 208, 200),  # Sanftes Grün
            QColor(196, 181, 253, 200),  # Sanftes Lila
            QColor(254, 215, 170, 200),  # Sanftes Orange
            QColor(252, 196, 25, 200),   # Sanftes Gelb
            QColor(248, 113, 113, 200),  # Sanftes Rot
        ]
        return random.choice(colors)
    
    def setup_animation_timer(self):
        """Timer für kontinuierliche Animationen einrichten"""
        self.animation_timer = QTimer()
        self.animation_timer.timeout.connect(self.update_particles)
        self.animation_timer.start(16)  # 60 FPS (1000ms / 60 ≈ 16ms)
    
    def update_particles(self):
        """Alle Partikel für die Animation aktualisieren"""
        for particle in self.particles:
            # Position aktualisieren
            particle['x'] += particle['speed_x']
            particle['y'] += particle['speed_y']
            particle['rotation'] += particle['rotation_speed']
            
            # Partikel am Bildschirmrand umschlagen lassen
            if particle['x'] < -100:
                particle['x'] = self.width() + 100
            elif particle['x'] > self.width() + 100:
                particle['x'] = -100
            
            if particle['y'] < -100:
                particle['y'] = self.height() + 100
            elif particle['y'] > self.height() + 100:
                particle['y'] = -100
            
            # Rotation auf 0-360 Grad begrenzen
            particle['rotation'] %= 360
        
        # Widget neu zeichnen
        self.update()
    
    def mouseMoveEvent(self, event):
        """Mausbewegungen verarbeiten und Partikel beeinflussen"""
        self.mouse_pos = event.pos()
        
        # Partikel in der Nähe der Maus leicht beeinflussen
        for particle in self.particles:
            distance = math.sqrt((particle['x'] - self.mouse_pos.x())**2 + 
                               (particle['y'] - self.mouse_pos.y())**2)
            
            if distance < 150:  # Einflussbereich der Maus
                # Partikel leicht zur Maus hin bewegen
                influence = (150 - distance) / 150 * 0.8
                dx = (self.mouse_pos.x() - particle['x']) * influence * 0.05
                dy = (self.mouse_pos.y() - particle['y']) * influence * 0.05
                
                particle['speed_x'] += dx
                particle['speed_y'] += dy
                
                # Geschwindigkeit begrenzen
                particle['speed_x'] = max(-3, min(3, particle['speed_x']))
                particle['speed_y'] = max(-3, min(3, particle['speed_y']))
        
        # Widget neu zeichnen
        self.update()
    
    def paintEvent(self, event):
        """Hintergrund mit allen Partikeln zeichnen"""
        painter = QPainter(self)
        painter.setRenderHint(QPainter.RenderHint.Antialiasing)
        
        # Sanften Hintergrund zeichnen
        background_gradient = QLinearGradient(0, 0, self.width(), self.height())
        background_gradient.setColorAt(0, QColor(248, 250, 252, 255))  # #f8fafc
        background_gradient.setColorAt(0.5, QColor(241, 245, 249, 255))  # #f1f5f9
        background_gradient.setColorAt(1, QColor(226, 232, 240, 255))  # #e2e8f0
        painter.fillRect(self.rect(), background_gradient)
        
        # Alle Partikel zeichnen
        for particle in self.particles:
            self.draw_particle(painter, particle)
    
    def draw_particle(self, painter: QPainter, particle: dict):
        """Einzelnes Partikel zeichnen"""
        painter.save()
        
        # Transparenz und Farbe setzen
        if particle['type'] == 'circle':
            color = particle['color']
            color.setAlpha(int(particle['opacity'] * 255))
            painter.setBrush(QBrush(color))
            painter.setPen(Qt.PenStyle.NoPen)
            
            # Kreis zeichnen
            painter.drawEllipse(
                int(particle['x'] - particle['size']/2),
                int(particle['y'] - particle['size']/2),
                int(particle['size']),
                int(particle['size'])
            )
        
        elif particle['type'] == 'blob':
            # Organische, fließende Form zeichnen
            color = particle['color']
            color.setAlpha(int(particle['opacity'] * 255))
            painter.setBrush(QBrush(color))
            painter.setPen(Qt.PenStyle.NoPen)
            
            # Blob-Form mit PainterPath erstellen
            path = QPainterPath()
            center_x, center_y = particle['x'], particle['y']
            size = particle['size']
            
            # Ersten Punkt setzen
            angle = particle['rotation'] * math.pi / 180
            first_x = center_x + size * math.cos(angle)
            first_y = center_y + size * math.sin(angle)
            path.moveTo(first_x, first_y)
            
            # Weitere Punkte für organische Form
            for i in range(1, particle['blob_points'] + 1):
                angle = (particle['rotation'] + i * 360 / particle['blob_points']) * math.pi / 180
                radius = size * (0.8 + 0.4 * math.sin(i * 0.5))  # Variierende Radien
                x = center_x + radius * math.cos(angle)
                y = center_y + radius * math.sin(angle)
                path.lineTo(x, y)
            
            path.closeSubpath()
            painter.drawPath(path)
        
        elif particle['type'] == 'gradient':
            # Große, subtile Gradienten zeichnen
            color1 = particle['color1']
            color2 = particle['color2']
            color1.setAlpha(int(particle['opacity'] * 255))
            color2.setAlpha(int(particle['opacity'] * 255))
            
            gradient = QLinearGradient(
                particle['x'] - particle['size']/2,
                particle['y'] - particle['size']/2,
                particle['x'] + particle['size']/2,
                particle['y'] + particle['size']/2
            )
            gradient.setColorAt(0, color1)
            gradient.setColorAt(1, color2)
            
            painter.setBrush(QBrush(gradient))
            painter.setPen(Qt.PenStyle.NoPen)
            
            # Gradient als Ellipse zeichnen
            painter.drawEllipse(
                int(particle['x'] - particle['size']/2),
                int(particle['y'] - particle['size']/2),
                int(particle['size']),
                int(particle['size'])
            )
        
        painter.restore()
    
    def resizeEvent(self, event):
        """Widget-Größe geändert - Partikel neu positionieren"""
        super().resizeEvent(event)
        # Partikel neu verteilen, wenn sich die Größe ändert
        self.redistribute_particles()
    
    def redistribute_particles(self):
        """Partikel neu über den verfügbaren Bereich verteilen"""
        for particle in self.particles:
            particle['x'] = random.uniform(0, self.width())
            particle['y'] = random.uniform(0, self.height())


class ElegantCard(QFrame):
    """Elegante Karten-Klasse für Bereiche mit Schatten"""
    
    def __init__(self, parent: Optional[QWidget] = None):
        super().__init__(parent)
        self.setup_style()
    
    def setup_style(self):
        """Karten-Styling für elegantes Aussehen"""
        self.setStyleSheet("""
            QFrame {
                background-color: #ffffff;
                border-radius: 20px;
                box-shadow: 0 4px 25px rgba(0, 0, 0, 0.08);
                border: 1px solid #f3f4f6;
            }
        """)


class RightPanel(QWidget):
    """Rechter Bereich mit allen Eingabefeldern - wird im Hauptfenster angezeigt"""
    
    def __init__(self, parent: Optional[QWidget] = None):
        super().__init__(parent)
        print("=== RightPanel wird erstellt ===")
        self.setup_ui()
        print("=== RightPanel Setup abgeschlossen ===")
    
    def setup_ui(self):
        """Benutzeroberfläche des rechten Bereichs einrichten"""
        # Hauptlayout - vertikal für bessere Übersicht
        main_layout = QVBoxLayout(self)
        main_layout.setSpacing(20)
        main_layout.setContentsMargins(20, 20, 20, 20)
        
        # Titel für den gesamten rechten Bereich
        title = ElegantLabel("Artikel-Details", "large")
        title.setAlignment(Qt.AlignmentFlag.AlignCenter)
        main_layout.addWidget(title)
        
        # Hauptbereich mit Eingabefeldern
        content_layout = QHBoxLayout()
        content_layout.setSpacing(30)
        
        # Linke Seite: Haupteingabefelder
        left_panel = self.create_main_fields()
        content_layout.addWidget(left_panel)
        
        # Rechte Seite: Materialfelder
        right_panel = self.create_material_fields()
        content_layout.addWidget(right_panel)
        
        main_layout.addLayout(content_layout)
    

    
    def create_main_fields(self) -> QWidget:
        """Haupteingabefelder erstellen"""
        panel = ElegantCard()
        panel.setMinimumWidth(400)  # Vergrößert von 280 auf 400
        
        layout = QVBoxLayout(panel)
        layout.setSpacing(30)  # Vergrößert von 20 auf 30
        layout.setContentsMargins(32, 32, 32, 32)  # Vergrößert von 24 auf 32
        
        # Titel für den Hauptbereich
        title = ElegantLabel("Artikel-Details", "large")
        title.setAlignment(Qt.AlignmentFlag.AlignCenter)
        layout.addWidget(title)
        
        # Eingabefelder erstellen und als Instanzvariablen speichern
        fields = [
            ("Material", "z.B. Stahl, Aluminium"),
            ("Durchmesser", "in mm"),
            ("Anzahl der Gebogenen", "Stück"),
            ("Sägelänge", "in mm")
        ]
        
        self.main_inputs = {}  # Dictionary für alle Eingabefelder
        
        # Einzelne Felder als Instanzvariablen für einfachen Zugriff
        self.material_field = None
        self.durchmesser_field = None
        self.anzahl_field = None
        self.saegelaenge_field = None
        
        for i, (label_text, placeholder) in enumerate(fields):
            # Label
            label = ElegantLabel(label_text, "medium")
            layout.addWidget(label)
            
            # Eingabefeld
            input_field = ElegantLineEdit(placeholder)
            input_field.setMinimumHeight(50)  # Mindesthöhe für bessere Sichtbarkeit
            self.main_inputs[label_text] = input_field
            
            # Einzelne Felder als Instanzvariablen speichern
            if i == 0:
                self.material_field = input_field
            elif i == 1:
                self.durchmesser_field = input_field
            elif i == 2:
                self.anzahl_field = input_field
            elif i == 3:
                self.saegelaenge_field = input_field
            
            layout.addWidget(input_field)
        
        return panel
    
    def create_material_fields(self) -> QWidget:
        """Rechte Seite mit Materialfeldern erstellen"""
        panel = ElegantCard()
        panel.setMinimumWidth(400)  # Vergrößert von 280 auf 400
        
        layout = QVBoxLayout(panel)
        layout.setSpacing(30)  # Vergrößert von 20 auf 30
        layout.setContentsMargins(32, 32, 32, 32)  # Vergrößert von 24 auf 32
        
        # Titel für den Materialbereich
        title = ElegantLabel("Material-Informationen", "large")
        title.setAlignment(Qt.AlignmentFlag.AlignCenter)
        layout.addWidget(title)
        
        # Navigationspfeil nach oben
        self.up_arrow_button = QPushButton("▲")
        self.up_arrow_button.setMinimumSize(80, 60)  # Vergrößert von 56x44 auf 80x60
        self.up_arrow_button.setStyleSheet("""
            QPushButton {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #9ca3af, stop:1 #6b7280);
                border: none;
                border-radius: 22px;
                color: white;
                font-weight: 600;
                font-size: 20px;
                font-family: "Segoe UI";
            }
            QPushButton:hover {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #d1d5db, stop:1 #9ca3af);
            }
            QPushButton:pressed {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #6b7280, stop:1 #4b5563);
            }
        """)
        self.up_arrow_button.clicked.connect(self.scroll_up)
        
        # Pfeil zentrieren
        arrow_layout = QHBoxLayout()
        arrow_layout.addStretch()
        arrow_layout.addWidget(self.up_arrow_button)
        arrow_layout.addStretch()
        layout.addLayout(arrow_layout)
        
        # 6 Materialfelder für zusätzliche Informationen
        self.material_inputs = []
        self.material_list_fields = []  # Liste für einfachen Zugriff
        
        for i in range(6):
            input_field = ElegantLineEdit(f"Material {i+1}")
            input_field.setMinimumHeight(45)  # Mindesthöhe für bessere Sichtbarkeit
            self.material_inputs.append(input_field)
            self.material_list_fields.append(input_field)  # Zur Liste hinzufügen
            layout.addWidget(input_field)
        
        # Navigationspfeil nach unten
        self.down_arrow_button = QPushButton("▼")
        self.down_arrow_button.setMinimumSize(80, 60)  # Vergrößert von 56x44 auf 80x60
        self.down_arrow_button.setStyleSheet("""
            QPushButton {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #9ca3af, stop:1 #6b7280);
                border: none;
                border-radius: 22px;
                color: white;
                font-weight: 600;
                font-size: 20px;
                font-family: "Segoe UI";
            }
            QPushButton:hover {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #d1d5db, stop:1 #9ca3af);
            }
            QPushButton:pressed {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #6b7280, stop:1 #4b5563);
            }
        """)
        self.down_arrow_button.clicked.connect(self.scroll_down)
        
        # Pfeil zentrieren
        arrow_layout = QHBoxLayout()
        arrow_layout.addStretch()
        arrow_layout.addWidget(self.down_arrow_button)
        arrow_layout.addStretch()
        layout.addLayout(arrow_layout)
        
        return panel
    
    def scroll_up(self):
        """Nach oben scrollen"""
        # Hier können Sie die Logik für das Navigieren nach oben implementieren
        pass
    
    def scroll_down(self):
        """Nach unten scrollen"""
        # Hier können Sie die Logik für das Navigieren nach unten implementieren
        pass
    

    
    def clear_all_fields(self):
        """Alle Eingabefelder leeren"""
        # Haupteingabefelder leeren
        for input_field in self.main_inputs.values():
            input_field.clear()
        
        # Materialfelder leeren
        for input_field in self.material_inputs:
            input_field.clear()
    
    def ensure_all_widgets_visible(self):
        """Alle Widgets des rechten Bereichs sichtbar machen"""
        # Liste aller Widgets im rechten Bereich (Textfelder + Materialliste + Pfeile)
        self.right_panel_widgets = [
            self.material_field,
            self.durchmesser_field,
            self.anzahl_field,
            self.saegelaenge_field,
            *self.material_list_fields,  # Alle Material-Listenfelder
            self.up_arrow_button,
            self.down_arrow_button
        ]
        
        # Alle Widgets explizit sichtbar machen
        for widget in self.right_panel_widgets:
            if widget:  # Prüfen ob Widget existiert
                widget.show()
                widget.setVisible(True)
        
        # Zusätzlich das gesamte Panel sichtbar machen
        self.show()
        self.setVisible(True)


class MainWindow(QMainWindow):
    """Elegantes Hauptfenster mit Apple-ähnlichem Design"""
    
    def __init__(self):
        super().__init__()
        self.setup_ui()
        self.setup_style()
        self.setup_animations()
    
    def setup_ui(self):
        """Benutzeroberfläche des Hauptfensters einrichten"""
        # Zentrales Widget mit interaktivem Hintergrund
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        # Interaktiven Hintergrund als erstes Element hinzufügen
        self.background = InteractiveBackground(central_widget)
        self.background.lower()  # Hintergrund hinter alle anderen Widgets setzen
        
        # Hauptlayout (horizontal) - links Artikelnummer, rechts der dynamische Bereich
        main_layout = QHBoxLayout(central_widget)
        main_layout.setSpacing(60)
        main_layout.setContentsMargins(60, 60, 60, 60)
        
        # Linke Seite: Artikelnummer-Bereich
        left_panel = self.create_article_section()
        main_layout.addWidget(left_panel)
        
        # Rechte Seite: Container für den dynamischen Bereich (anfangs unsichtbar)
        self.right_panel_container = QWidget()
        self.right_panel_container.setMinimumWidth(900)  # Vergrößert von 600 auf 900
        self.right_panel_container.setMaximumWidth(900)  # Vergrößert von 600 auf 900
        main_layout.addWidget(self.right_panel_container)
        self.right_panel_container.hide()  # Anfangs unsichtbar
        
        # Hintergrund an die Größe des zentralen Widgets anpassen
        self.background.setGeometry(central_widget.rect())
    
    def create_article_section(self) -> QWidget:
        """Artikelnummer-Bereich mit Eingabefeld und Button erstellen"""
        article_card = ElegantCard()
        article_card.setMaximumWidth(500)
        
        layout = QVBoxLayout(article_card)
        layout.setSpacing(32)
        layout.setContentsMargins(48, 48, 48, 48)
        
        # Titel für den linken Bereich
        title = ElegantLabel("Hydraulik Rohre", "large")
        title.setAlignment(Qt.AlignmentFlag.AlignCenter)
        layout.addWidget(title)
            # Flexibler Platz oben
        layout.addStretch()
        # Logo unter dem Titel
        logo_label = QLabel()
        logo_pixmap = QPixmap(r"C:\Users\admin\Desktop\Python lernen\Logo.png")
        logo_pixmap = logo_pixmap.scaled(180, 180, Qt.AspectRatioMode.KeepAspectRatio, Qt.TransformationMode.SmoothTransformation)
        logo_label.setPixmap(logo_pixmap)
        logo_label.setAlignment(Qt.AlignmentFlag.AlignCenter)
        logo_label.setStyleSheet("border: none; background: transparent;")  # Kein Rahmen, transparenter Hintergrund
        layout.addWidget(logo_label)

    
        
  
        
        # Artikelnummer-Eingabefeld
        self.article_input = ElegantLineEdit("Artikelnummer eingeben...")
        self.article_input.setMinimumWidth(300)   # Mindestbreite, z.B. 300px
        self.article_input.setMaximumWidth(400)   # Optional Maximalbreite
        self.article_input.textChanged.connect(self.on_article_changed)

        # In einem horizontalen Layout zentrieren
        input_layout = QHBoxLayout()
        input_layout.addStretch()               
        input_layout.addWidget(self.article_input)
        input_layout.addStretch()               
        layout.addLayout(input_layout)


        
        # Ablesen-Button
        self.read_button = ElegantButton("Ablesen", primary=True)
        self.read_button.clicked.connect(self.show_right_panel)
        layout.addWidget(self.read_button)
        
        # Button zentrieren
        button_layout = QHBoxLayout()
        button_layout.addStretch()
        button_layout.addWidget(self.read_button)
        button_layout.addStretch()
        
        # Layout anpassen
        layout.removeWidget(self.read_button)
        layout.addLayout(button_layout)
        
        # Flexibler Platz unten
        layout.addStretch()
        

        
        return article_card
    
    def setup_style(self):
        """Hauptfenster-Styling für elegantes Aussehen"""
        # Kein statischer Hintergrund, da wir den interaktiven Hintergrund haben
        self.setStyleSheet("""
            QMainWindow {
                background: transparent;
            }
        """)
        
        # Fenster-Eigenschaften - angepasst für das neue Layout
        self.setWindowTitle("Hydraulik_Rohre")
        self.setMinimumSize(1000, 700)  # Reduziert von 1400x900 auf 1000x700
        self.resize(1200, 800)  # Reduziert von 1600x1000 auf 1200x800
        
        # Fenster zentrieren
        self.center_window()
    
    def setup_animations(self):
        """Fenster-Animationen einrichten"""
        # Sanfte Einblend-Animation
        self.setWindowOpacity(0.0)
        self.fade_in_animation = QPropertyAnimation(self, b"windowOpacity")
        self.fade_in_animation.setDuration(500)
        self.fade_in_animation.setStartValue(0.0)
        self.fade_in_animation.setEndValue(1.0)
        self.fade_in_animation.setEasingCurve(QEasingCurve.Type.OutCubic)
    
    def showEvent(self, event):
        """Fenster wird angezeigt - Animation starten"""
        super().showEvent(event)
        self.fade_in_animation.start()
    
    def resizeEvent(self, event):
        """Fenster-Größe geändert - Hintergrund anpassen"""
        super().resizeEvent(event)
        # Hintergrund an die neue Fenstergröße anpassen
        if hasattr(self, 'background'):
            self.background.setGeometry(self.centralWidget().rect())
    
    def center_window(self):
        """Fenster auf dem Bildschirm zentrieren"""
        screen = QApplication.primaryScreen().geometry()
        x = (screen.width() - self.width()) // 2
        y = (screen.height() - self.height()) // 2
        self.move(x, y)
    
    def on_article_changed(self, text: str):
        """Wird aufgerufen, wenn sich die Artikelnummer ändert"""
        # Wenn der rechte Bereich sichtbar ist, alle Felder leeren
        if hasattr(self, 'right_panel') and self.right_panel_container.isVisible():
            self.right_panel.clear_all_fields()
    
    def show_right_panel(self):
        """Rechten Bereich im Hauptfenster anzeigen"""
        print("=== show_right_panel wird aufgerufen ===")
        
        # Prüfen, ob Artikelnummer eingegeben wurde
        article_number = self.article_input.text().strip()
        print(f"Artikelnummer: '{article_number}'")
        
        if not article_number:
            # Elegante Warnung anzeigen
            self.show_warning("Bitte geben Sie eine Artikelnummer ein!")
            return
        
        # Rechten Bereich erstellen falls noch nicht vorhanden
        if not hasattr(self, 'right_panel'):
            print("Erstelle neuen RightPanel...")
            self.right_panel = RightPanel()
            right_layout = QVBoxLayout(self.right_panel_container)
            right_layout.setContentsMargins(0, 0, 0, 0)
            right_layout.addWidget(self.right_panel)
            print("RightPanel erstellt und zum Layout hinzugefügt")
        else:
            print("RightPanel existiert bereits")
        
        print(f"RightPanel Container sichtbar vor show(): {self.right_panel_container.isVisible()}")
        print(f"RightPanel sichtbar vor show(): {self.right_panel.isVisible()}")
        
        # Alle Widgets im rechten Bereich sichtbar machen
        self.right_panel.ensure_all_widgets_visible()
        
        # Rechten Bereich anzeigen - OHNE Animation für bessere Sichtbarkeit
        self.right_panel_container.show()
        self.right_panel.show()
        
        # Größe explizit setzen
        self.right_panel_container.setMinimumHeight(700)  # Vergrößert von 600 auf 700
        self.right_panel_container.setMaximumHeight(700)  # Vergrößert von 600 auf 700
        
        print(f"RightPanel Container sichtbar nach show(): {self.right_panel_container.isVisible()}")
        print(f"RightPanel sichtbar nach show(): {self.right_panel.isVisible()}")
        print(f"RightPanel Container Größe: {self.right_panel_container.size()}")
        print(f"RightPanel Größe: {self.right_panel.size()}")
        
        # Kurze Verzögerung für bessere Sichtbarkeit
        QTimer.singleShot(100, lambda: self.force_update_layout())
        
        print("=== show_right_panel abgeschlossen ===")
    
    def force_update_layout(self):
        """Layout des rechten Bereichs erzwungen aktualisieren"""
        print("=== force_update_layout wird aufgerufen ===")
        if hasattr(self, 'right_panel'):
            self.right_panel.update()
            self.right_panel.repaint()
            self.right_panel_container.update()
            self.right_panel_container.repaint()
            print("Layout-Update abgeschlossen")
    
    def show_warning(self, message: str):
        """Elegante Warnung anzeigen"""
        # Warnung durch Ändern des Button-Texts und Styling
        original_text = self.read_button.text()
        self.read_button.setText("⚠ " + message)
        self.read_button.setStyleSheet("""
            QPushButton {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #f87171, stop:1 #ef4444);
                border: none;
                border-radius: 26px;
                color: white;
                padding: 16px 32px;
                font-weight: 500;
                font-size: 15px;
            }
        """)
        
        # Nach 3 Sekunden zurücksetzen
        QTimer.singleShot(3000, lambda: self.reset_button(original_text))
    
    def reset_button(self, original_text: str):
        """Button auf ursprünglichen Zustand zurücksetzen"""
        self.read_button.setText(original_text)
        self.read_button.setup_style()
    
    def show_success_message(self, message: str):
        """Erfolgsmeldung anzeigen"""
        # Erfolgsmeldung durch Ändern des Button-Texts und Styling
        original_text = self.read_button.text()
        self.read_button.setText("✓ " + message)
        self.read_button.setStyleSheet("""
            QPushButton {
                background: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                    stop:0 #10b981, stop:1 #059669);
                border: none;
                border-radius: 26px;
                color: white;
                padding: 16px 32px;
                font-weight: 500;
                font-size: 15px;
            }
        """)
        
        # Nach 3 Sekunden zurücksetzen
        QTimer.singleShot(3000, lambda: self.reset_button(original_text))


def main():
    """Hauptfunktion der Anwendung"""
    # Qt-Anwendung erstellen
    app = QApplication(sys.argv)
    
    # Anwendungseigenschaften setzen
    app.setApplicationName("Hydraulik_Rohre")
    app.setApplicationVersion("2.0")
    app.setOrganizationName("Meine Firma")
    
    # Hauptfenster erstellen und anzeigen
    main_window = MainWindow()
    main_window.show()
    
    # Event-Loop starten
    sys.exit(app.exec())


if __name__ == "__main__":
    main()
