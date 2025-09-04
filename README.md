# Manda1-
Contractors Estimator 
import React, { useState, useEffect, useRef } from 'react';
import { Calculator, Home, Wrench, Palette, TreePine, FileText, Clock, DollarSign, Ruler, Lightbulb, Minus, Download, Eye, EyeOff, Calendar, Shield, Camera, Users, Phone, MapPin, Star, MessageCircle, Globe, Image as ImageIcon, Mic, Video, Settings } from 'lucide-react';

const Manda1ContractorsEstimator = () => {
  const [activeTab, setActiveTab] = useState('overview');
  const [units, setUnits] = useState('imperial');
  const [language, setLanguage] = useState('en');
  const [projectData, setProjectData] = useState({
    projectName: '',
    clientName: '',
    address: '',
    startDate: '',
    estimatedDays: '',
    laborRate: 25
  });
  
  const [categories, setCategories] = useState({
    materials: [],
    labor: [],
    tools: [],
    permits: [],
    fixtures: [],
    landscaping: [],
    plumbing: []
  });

  const [showAI, setShowAI] = useState(false);
  const [safetyChecklist, setSafetyChecklist] = useState([]);
  const [clientAppointments, setClientAppointments] = useState([]);
  const [userProfile, setUserProfile] = useState({
    name: '',
    company: '',
    license: '',
    specialties: [],
    reviews: []
  });

  // Welcome announcement on component mount
  useEffect(() => {
    const message = new SpeechSynthesisUtterance("Welcome to Manda One. Taking the stress out of estimates.");
    window.speechSynthesis.speak(message);
  }, []);

  const laborCategories = [
    { name: "Demolition", rate: 30 },
    { name: "Drywall Installation", rate: 40 },
    { name: "Painting", rate: 35 },
    { name: "Tiling", rate: 50 },
    { name: "Plumbing", rate: 85 },
    { name: "Electrical", rate: 75 },
    { name: "Roofing", rate: 65 },
    { name: "Landscaping Labor", rate: 28 },
    { name: "Flooring Installation", rate: 45 },
    { name: "Kitchen Cabinet Install", rate: 55 },
    { name: "HVAC Work", rate: 90 },
    { name: "Concrete Work", rate: 48 }
  ];

  const materialLibrary = {
    flooring: {
      'Ceramic Tiles': { unit: 'sq ft', price: 3.50, sources: ['Home Depot: $3.50', 'Lowes: $3.45', 'Floor & Decor: $3.25'] },
      'Hardwood Oak': { unit: 'sq ft', price: 8.99, sources: ['Lumber Liquidators: $8.99', 'Home Depot: $9.25'] },
      'Laminate': { unit: 'sq ft', price: 2.25, sources: ['Costco: $2.25', 'Home Depot: $2.50', 'Lowes: $2.35'] },
      'Vinyl Planks': { unit: 'sq ft', price: 4.75, sources: ['Home Depot: $4.75', 'Lowes: $4.85', 'Menards: $4.60'] }
    },
    concrete: {
      'Ready Mix Concrete': { unit: 'cubic yard', price: 125, sources: ['Local Concrete Co: $125', 'Home Depot: $140'] },
      'Portland Cement': { unit: '94lb bag', price: 12.50, sources: ['Home Depot: $12.50', 'Lowes: $12.75', 'Ace Hardware: $13.00'] },
